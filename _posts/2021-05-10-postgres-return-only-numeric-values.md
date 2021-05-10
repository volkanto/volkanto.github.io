---
layout: post
title: "Postgresql - Return only numeric values from column"
description: "How to return only numeric values from specific column on Postgresql"
category: postgresql
tags: postgresql postgres-tricks tips
---

### Return only numberic values from column

If you ever need to fetch the numeric values from a column, below query can help to achieve. The query was only tested with Postgresql and works like a charm.

```sql
select substring([COLUMN-NAME] FROM '[0-9]+') from [TABLE-NAME]
```

Reference: [Stackoverflow](https://stackoverflow.com/questions/21946657/return-only-numeric-values-from-column-postgresql)
