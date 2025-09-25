## Create Workbook
```r
wb <- createWorkbook()
```

## Add Worksheet
```r
addWorksheet(
    wb,
    sheet = "Sheet1", # name for the new worksheet
    gridLines = FALSE # hide worksheet grid lines
    )
```

## Write Data

```r

floor_date(Sys.Date(), unit = "week") %>% {
    this_wed <<- format(`+`(., days(3)), "%B %d, %Y")
    this_thur <<- format(`+`(., days(4)), "%B %d, %Y")
}

writeData(
    wb,
    sheet = "Sheet1",
    paste0(
        "The summary for Thursday, ",
        this_thur,
        " can be found below and includes data up to end-of-day ",
        this_wed,
        "."
    ),
    startCol = 1,
    startRow = i
)
```

## Add Style

```r

title_style <- createStyle(
    fontSize = 15,
    textDecoration = c("bold", "underline"),
    fontColour = "#0F0E0E",
    bgFill = "white",
    halign = "left", # Horizontal alignment of cell contents
    valign = "center", # Vertical alignment of cell contents
    wrapText = FALSE # If TRUE cell contents will wrap to fit in column.
)

addStyle(
    wb,
    sheet = "Sheet1",
    style = title_style,
    rows = i,
    cols = 1
)
```

## Set the width of the column
```r
setColWidths(wb, sheet = "Sheet1", cols = 1:2, widths = 32)
```

## Set up the time format
```r
options("openxlsx.datetimeFormat" = "yyyy-mm-dd")
```

## Save Workbook
```r
saveWorkbook(
    wb,
    "test.xlsx",
    overwrite = TRUE
    )
```