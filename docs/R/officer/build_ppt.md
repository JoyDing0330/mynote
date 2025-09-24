## Create ppt template
1. `View` > `Master Views` > `Slide Master`
2. `Slide Master` > `Insert Slide Master`
3. `Insert Placeholder`
4. `Home` > `Editing` > `Select` > `Selection panel` > rename/move/delete

## Create slide
- Create blank slide
```r
doc <- read_pptx()
```

- Read in template
```r
doc <- read_pptx(path = here("template.pptx"))
```
## Add slide
```r
doc <- add_slide(
    doc,
    layout = "Slide1", # layout name in the template
    master = "Office Theme"
    )
```

## Add text/ggplot/flextable in the slide

- Use template to locate
```r
doc <- ph_with(
    doc,
    value = format(Sys.Date(), "%B %d, %Y"),
    location = ph_location_label("date") # placeholder name in the template
    )
```

- Use index to locate
```r
ph_with(
    value = indicator_ftbl$ed_visit,
    location = ph_location(left = 0.36, top = 1.3)
)
```

## Add external image
```r
ph_with(
    value = external_img("plot_.png"), # width = 9.2, height = 5.6
    location = ph_location_label("content"),
    use_loc_size = FALSE # if set to FALSE, external_img width and height will be used
    )
```

## Add hyperlink

```r
  link_text <- fpar(
    ftext("xxx ("),
    hyperlink_ftext(text = "Link", href = "https://xxxx"),
    ftext(")")
  )
```
## Add bullet points

```r
  bullet_points <- unordered_list(
    c(
      "A",
      "B",
      "bbb",
      "C"
    ),
    level_list = c(1, 1, 2, 1)
  )
```

## Export
```r
print(
  ppt,
  here::here("test.pptx")
  )
```