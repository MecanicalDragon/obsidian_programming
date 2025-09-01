The main drawback of the `OFFSET` is database must iterate over all the entries containing the offset anyway, and the larger the offset, the greater waste work is.

The other issue with the `OFFST` is between two requests data may change, and rows my be skipped if records from offset were removed or duplicated if they were added

The better solution is using [[SQL WHERE & HAVING|WHERE]] instead of `OFFSET` like below:
`SELECT * FROM my_table WHERE dts > ? ORDER BY dts LIMIT ?`
The idea is to keep the latest used for the sorting key and use it in `WHERE` clause. Anyway, you have to avoid duplication and skipping on the edges of the pages. Sorting keys may be not unique, and you have to detect records with the same keys and handle these scenarios on the page borders (the code example is below)
```
int sameLastDtsCounter = 0;  
while (rs.next()) {  
  result.add(converter.convert(rs));  
  long currentDts = rs.getLong("dts");  
  if (currentDts == latestFetchedDts) {  
    sameLastDtsCounter++;  
  } else {  
    latestFetchedDts = currentDts;  
    sameLastDtsCounter = 1;  
  }  
}  
if (resultSizeBeforePageFetch + PAGE_SIZE == result.size()) {  
  for (int i = 0; i < sameLastDtsCounter; i++) {  
    result.remove(result.size() - 1);  
  }  
  latestFetchedDts--;  
} else {  
  nextIterationNeeded = false;  
}
```

