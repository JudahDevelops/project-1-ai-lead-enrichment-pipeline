# Temp Fix — Build Prompt + Mark Unverified

## Build Prompt node

**chatInput**
```
={{ 'Write ONE cold email opening line (max 20 words) for ' + $json.name + ', ' + $json.title + ' at ' + $json.company + ' (' + $json.industry + ' industry, ' + $json.employee_count + ' employees). Reference their role or growth stage. No fluff, no buzzwords. Output the line only.' }}
```

**name**
```
={{ $json.name }}
```

**title**
```
={{ $json.title }}
```

**company**
```
={{ $json.company }}
```

**industry**
```
={{ $json.industry }}
```

**email**
```
={{ $json.email }}
```

**employee_count**
```
={{ $json.employee_count }}
```

---

## Mark Unverified node

**name**
```
={{ $json.name }}
```

**title**
```
={{ $json.title }}
```

**company**
```
={{ $json.company }}
```

**industry**
```
={{ $json.industry }}
```

**email**
```
={{ $json.email }}
```

**email_status**
```
unverified
```

**employee_count**
```
={{ $json.employee_count }}
```

**icebreaker**
```
SKIPPED
```

**enriched_at**
```
={{ $now.toISO() }}
```
