# Load required libraries
library(ggplot2)
library(ggpmisc)   # For stat_poly_eq
library(ggpubr)    # If needed for other pub-ready features; remove if unused
library(patchwork) # For combining plots with +

# Import data (assuming Sugar_Contents is already loaded/defined)
sc <- data.frame(
  day = Sugar_Contents$Day,
  LD = Sugar_Contents$LD,
  YDWM = Sugar_Contents$YDWM,
  ZM2 = Sugar_Contents$ZM2,
  YC = Sugar_Contents$YC,
  HY4 = Sugar_Contents$HY4
)

# Function to create a single plot for a given y-variable (sample)
create_sample_plot <- function(data, y_var) {
  ggplot(data, aes(x = day, y = .data[[y_var]])) +  # Use .data[[ ]] for dynamic y
    geom_point(size = 3, color = "red", alpha = 0.3) +  # No redundant data/aes
    labs(
      x = "Day",
      y = expression(bold(Content) ~ mu * g)  # Cleaner expression
    ) +
    geom_smooth(
      method = "loess",  # Locally weighted regression
      se = FALSE,
      size = 1,
      span = 0.8
    ) +
    stat_poly_eq(
      formula = y ~ poly(x, 3, raw = TRUE),
      aes(label = paste(..eq.label.., sep = "~~~")),
      parse = TRUE,
      label.x.npc = "right",
      vjust = 0.1
    ) +
    stat_poly_eq(
      formula = y ~ poly(x, 3, raw = TRUE),
      aes(label = paste(..rr.label.., ..adj.rr.label.., sep = "~~~")),  # R² and adj R²
      parse = TRUE,
      label.x.npc = "right",
      vjust = 1.1
    ) +
    stat_poly_eq(
      formula = y ~ poly(x, 3, raw = TRUE),
      aes(label = paste(..AIC.label.., ..BIC.label.., sep = "~~~")),
      parse = TRUE,
      label.x.npc = "right",
      vjust = 3.2
    ) +
    theme_classic() +  # Start with classic theme
    theme(
      axis.title = element_text(
        family = "sans",  # Font family
        face = "bold",    # Bold
        size = 15,        # Size
        lineheight = 1    # Line spacing
      ),
      axis.text = element_text(
        family = "sans",  # Font family
        size = 15         # Size
      )
    )
}

# List of sample columns (in reverse order to match your SCP1 to SCP5)
samples <- c("LD", "YDWM", "ZM2", "YC", "HY4")

# Generate list of plots
sample_plots <- lapply(samples, function(y) create_sample_plot(sc, y))

# Assuming STDS is defined elsewhere (e.g., another plot); replace as needed
# If STDS is not needed, remove it and start with sample_plots[[1]] + ...
# For example: mosaic2 <- sample_plots[[1]] + sample_plots[[2]] + ... 
mosaic2 <- STDS + sample_plots[[1]] + sample_plots[[2]] + sample_plots[[3]] + sample_plots[[4]] + sample_plots[[5]]

# To arrange in a grid (e.g., 2 rows), add: mosaic2 + plot_layout(ncol = 3)
# Display the combined plot
mosaic2
