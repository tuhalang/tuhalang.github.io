---
layout: wiki
title: Cutting emoij text in java
cate1: java
cate2: 
description: java, emoij
keywords: java, emoij
type:
link:
---

Have you had to cut a composite text included special text, emoij, etc. in Java yet? How do you handle it? In this artical I will share my experience with it.

```java
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

import lombok.AccessLevel;
import lombok.NoArgsConstructor;

@NoArgsConstructor(access = AccessLevel.PRIVATE)
public class StringUtils {

    /**
     * Get n characters from index 0
     *
     * @param text
     * @param amount
     * @return
     */
    public static String getCharacters(String text, int amount) {
        if (amount <= 0) {
            return "";
        }

        if (amount >= text.length() || amount >= text.codePointCount(0, text.length())) {
            return text;
        }

        // find all special characters
        final Pattern unicodeOutliers = Pattern.compile("[^\\x00-\\xFF]",
                                                        Pattern.UNICODE_CASE | Pattern.CASE_INSENSITIVE);
        final Matcher unicodeOutlierMatcher = unicodeOutliers.matcher(text);
        final List<String> matchList = new ArrayList<>();
        while (unicodeOutlierMatcher.find()) {
            matchList.add(unicodeOutlierMatcher.group());
        }

        // find indexes start and end of special characters
        final Map<Integer, Integer> mapEntry = new HashMap<>();
        int fromIndex = 0;
        for (int i = 0; i < matchList.size(); i++) {
            final String temp = matchList.get(i);
            final int start = text.indexOf(temp, fromIndex);
            final int end = start + temp.length();
            fromIndex = end;
            mapEntry.put(start, end);
        }

        // find cutting point
        int count = 0;
        fromIndex = 0;
        while (fromIndex < text.length() && count < amount) {
            if (mapEntry.containsKey(fromIndex)) {
                fromIndex = mapEntry.get(fromIndex);
            } else {
                fromIndex++;
            }
            count++;
        }

        return text.substring(0, fromIndex);
    }

    /**
     * Substring form index 0 to index
     *
     * @param text
     * @param index
     * @return
     */
    public static String subString(String text, int index) {
        if (index < 0 || index > text.length()) {throw new IllegalStateException("Index out of bounds!");}

        final int range = 5;
        final int startSplit = Math.max(index - range, 0);
        final int endSplit = Math.min(index + range, text.length());

        // spilt sub string in range need to cut
        //    start   index    end
        // ----||-------|-------||----
        final String subText = text.substring(startSplit, endSplit);

        // keep string to UTF8 format
        final byte[] utf8Bytes = subText.getBytes(StandardCharsets.UTF_8);
        final String utf8SubText = new String(utf8Bytes, StandardCharsets.UTF_8);

        // find all special characters
        final Pattern unicodeOutliers = Pattern.compile("[^\\x00-\\xFF]",
                                                        Pattern.UNICODE_CASE | Pattern.CASE_INSENSITIVE);
        final Matcher unicodeOutlierMatcher = unicodeOutliers.matcher(utf8SubText);
        final List<String> matchList = new ArrayList<>();
        while (unicodeOutlierMatcher.find()) {
            matchList.add(unicodeOutlierMatcher.group());
        }

        // find indexes start and end of special characters
        final List<Entry> listEntry = new ArrayList<>();
        int fromIndex = 0;
        for (int i = 0; i < matchList.size(); i++) {
            final String temp = matchList.get(i);
            final int start = subText.indexOf(temp, fromIndex);
            final int end = start + temp.length();
            fromIndex = end;
            listEntry.add(new Entry(start, end));
        }

        // find suitable index to cut string
        final int indexHandled = getIndex(listEntry, index - startSplit);
        return text.substring(0, startSplit + indexHandled);
    }

    private static int getIndex(List<Entry> list, int index) {
        for (Entry entry : list) {
            if (index <= entry.start) {
                // special characters not in cutting position
                break;
            } else if (index < entry.end) {
                // special characters in cutting position, adjust the position to end of the previous
                // character to avoid error
                index = entry.start;
                break;
            }
        }
        return index;
    }
}

class Entry {
    int start;
    int end;

    public Entry(int start, int end) {
        this.start = start;
        this.end = end;
    }
}
```