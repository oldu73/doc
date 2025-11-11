# outlook

---

## Advanced Search in Outlook: Logical Operators and Limitations

Outlook supports simple **logical operators** such as `AND`, `OR`, and
`NOT`, as well as **field-based filters** for email searches. However,
it **does not support full regular expressions (regex)**.

### 1. Logical Operators Supported

You can use basic Boolean operators in the search bar:

Example / Effect
  
`from:olivier AND subject:invoice` / Emails sent by *Olivier* **and** containing *invoice* in the subject

`from:olivier OR from:pierre` / Emails from *Olivier* **or** *Pierre*

`subject:(report OR summary)` / Subject contains *report* **or** *summary*

`NOT from:newsletter` / Excludes emails from *newsletter*
  
> Operators must be written in **uppercase** (`AND`, `OR`, `NOT`)  
> Parentheses `()` can also be used for grouping expressions.

### 2. Searchable Fields

You can combine operators with **specific search fields**:

- `from:` → sender  
- `to:` → recipient  
- `subject:` → email subject  
- `body:` → message content  
- `hasattachment:yes` → messages with attachments  
- `received:>=01/11/2025` → received since a specific date

Example:

``` text
(from:olivier OR from:pierre) AND subject:("monthly report") AND hasattachment:yes
```

### 3. Regular Expressions (Regex) -- Not Supported ❌

Outlook **does not support regex patterns** such as `subject:/invoice\s+\d+/`.

Instead, you can use **wildcards** for partial matches:

Example / Matches

`subject:invoice*` / Finds *invoice*, *invoices*, *invoice123*, etc.
  
`from:o?ivier` / Matches *olivier* and *omivier*, for example

### 4. Pro Tips

For **more advanced searches** (regex, automation, etc.), you can:

- Export or index your emails and use **Python** or **PowerShell scripts**  
- Use **Microsoft 365 Search API**, which provides more flexible query capabilities

---
