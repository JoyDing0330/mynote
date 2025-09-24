## Setup text formatting properties

- Title property
```r
title_style <- fp_text(
    font.size = 10.5,
    font.family = "Arial",
    bold = TRUE
    )
```

- Superscript property
```r
sup_style <- fp_text(
    vertical.align = "superscript",
    font.family = "Arial",
    font.size = 10
    )
```

## Create Word documents

- Create blank doc
```r
doc <- read_docx()
```

- Read in template
```r
doc <- read_docx(path = here("template.docx"))
```

## Insert text

- Prepare formatted text with properties
  ```r
  # Title
    table1_title <- fpar(
    ftext("Table 1. ", prop = bold_style),
    ftext(
        paste0(
        "Number of cases (n=",
        nrow(df),
        ")"
        ),
        prop = title_font
    ),
    fp_p = fp_par(padding.bottom = 10) # add padding to the bottom
    )

  # Footnote
    fig2_footnote <- fpar(
    ftext("1", prop = sup_style),
    ftext(
        " footnote placeholder",
        prop = footnote_style
    ),
    run_linebreak(), # line break
    ftext("2", prop = sup_style),
    ftext(
        " footnote placeholder",
        prop = footnote_style
    )
    )
  ```



- Insert formatted text into Word doc
```r
doc <- body_add_fpar(
  doc,
  value = table1_title
)
```

- Insert an empty line
```r
doc <- body_add_par(doc, "")
```

- Insert multiple empty lines
```r
# Or use function

add_empty_lines <- function(doc, n) {
  for (i in seq_len(n)) {
    doc <- body_add_par(doc, "")
  }
  doc
}

doc <- add_empty_lines(doc, 3)
```

## Insert flextable

```r
doc <- body_add_flextable(doc, ftbl)
```

## Insert ggplot figures

```r
doc <- body_add_gg(
    doc,
    value = ggplot,
    width = 6.5,
    height = 3.8,
    res = 300,
    style = "centered"
    )
```

## Add page break

```r
doc <- body_add_break(doc)
```

## Move cursor

- Set the cursor at the beginning of the document, on the first element of the document
```r
doc <- cursor_begin(doc)
```
- Set the cursor at a bookmark that has previously been set
```r
doc <- cursor_bookmark(doc, "second_page")
```

## Export doc
```r
  print(
    doc,
    "test.docx"
  )
```

