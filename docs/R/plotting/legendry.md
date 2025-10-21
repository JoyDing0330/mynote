## Reference
[Legendry guide](https://teunbrand.github.io/legendry/reference/guide_axis_nested.html)

## Create nested axis

### Type
- [Stack composition](https://teunbrand.github.io/legendry/articles/guide_composition.html)
- [Line](https://teunbrand.github.io/legendry/reference/primitive_line.html)
- [Ticks](https://teunbrand.github.io/legendry/reference/primitive_ticks.html)
- [Labels](https://teunbrand.github.io/legendry/reference/primitive_labels.html)
- [Brackets](https://teunbrand.github.io/legendry/reference/primitive_bracket.html)
- [Box](https://teunbrand.github.io/legendry/reference/primitive_box.html)
- [Fence](https://teunbrand.github.io/legendry/reference/primitive_fence.html)

### Steps
1. Create interactions
    ```r
    data_interaction <- mutate(
        data,
        ymw = interaction(
            format(date, "%U"), # week of the year
            format(date, "%b"), # month of the year
            lubridate::year(date) # year
            )
    )
    ```
2. Aggange the data by date
    ```r
    data_arranged <- arrange(
        data_interaction,
        date
    )
    ```
3. Factoriza and relevel the data
    ```r
    relevel <- unique(data_arranged[["ymw"]])
    data_arranged$ymw <- factor(data_arranged$ymw, levels = relevel)
    ```
4. Plot the data
      1. Use the interaction column as the x axis
    ```r
    plot <- ggplot(
        data = data_arranged,
        aes(x = ymw)
        ) +
        # add any layers as you want (e.g., bars, lines, dots ...)
        ...
    
    ```
5. Add fence or brackets
      1. Add fence
    ```r
    p + guides(
      x = primitive_fence(rail = "inner", drop_zero = FALSE)
      )
    ```
      1. Add brackets
    ```r
    p + guides(
        x = legendry::guide_axis_nested(drop_zero = FALSE)
        )
    ```
6. Set up the themes
      1. Set up line width
        ```r
        p + theme_guide(fence = element_line(linewidth = 0.15))
        ```
      2. Set up line color
        ```r
        p + theme_guide(bracket = element_line("red", linewidth = 1))
        ```
      3. Set up the fence post and fence rail color separately
        ```r
          p + theme_guide(
            fence.post = element_line("tomato"),
            fence.rail = element_line("dodgerblue")
            )
        ```


### Tips
1. Don't drop nesting indicators that have 0-width
```r
p + guides(x = guide_axis_nested(drop_zero = FALSE))
```
2. Change bracket type
```r
p + guides(x = guide_axis_nested(bracket = "curvy"))
```
3. Add second x-axis
```r
p + guides(x.sec = guide_axis_nested(drop_zero = FALSE))
```
