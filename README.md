# 📌 ETL Project: Indonesian Liga 1 Player Data

## 📝 Overview
This project implements an **ETL (Extract, Transform, Load) pipeline** using **R** to process Indonesian Liga 1 player data. The dataset undergoes extraction, cleaning, transformation, and is then saved in a structured format for further analysis.

## 📂 Dataset Details
The dataset contains detailed information about **Liga 1 football players**. Key attributes include:

| **Variable**  | **Type**  | **Description** |
|--------------|----------|----------------|
| `player_id`  | Integer  | Unique player identifier |
| `name`       | Character | Full name |
| `birth_date` | Date     | Date of birth |
| `club`       | Character | Club name |
| `position`   | Character | Playing position |
| `nationality` | Character | Nationality |
| `height`     | Numeric  | Height in cm |
| `weight`     | Numeric  | Weight in kg |

## 🔄 ETL Process

### 1️⃣ Extract (Data Loading & Backup)
- Reads raw player data from `player_liga1.csv`.
- Creates a **backup** if enabled in the configuration.

```r
library(readr)
data <- read_csv(config$input_path)

if (config$backup_enabled) {
  file.copy(config$input_path, paste0(config$input_path, "_backup.csv"))
}
```

### 2️⃣ Transform (Data Cleaning & Feature Engineering)
- Converts `birth_date` to **Date format**.
- Removes **duplicates** and handles **missing values**.
- Computes **player age** dynamically.

```r
library(dplyr)
library(lubridate)

data_clean <- data %>%
  distinct() %>%
  mutate(
    birth_date = ymd(birth_date),
    age = floor(interval(start = birth_date, end = Sys.Date()) / years(1))
  ) %>%
  replace_na(list(nationality = "Unknown"))
```

### 3️⃣ Load (Saving Processed Data)
- Saves the cleaned dataset as `cleaned_player_liga1.csv`.

```r
write_csv(data_clean, config$output_path)
```

## 📊 Data Visualization
A **histogram of player age distribution** is generated using `ggplot2`.

```r
library(ggplot2)

ggplot(data_clean, aes(x = age)) +
  geom_histogram(binwidth = 1, fill = "blue", alpha = 0.7) +
  theme_minimal() +
  labs(title = "Age Distribution of Liga 1 Players", x = "Age", y = "Count")
```

## 🎯 Key Features
✅ Extracts **Liga 1 player data** from CSV files  
✅ Cleans and structures **birth dates, nationality, and duplicates**  
✅ Computes **player age** dynamically  
✅ Saves **cleaned dataset** for further analysis  
✅ Includes **data visualization** for insights  

## 🚀 Getting Started

### 📌 Prerequisites
Ensure you have **R** and the required packages installed:
```r
install.packages(c("dplyr", "lubridate", "readr", "ggplot2", "R.utils"))
```

### 📌 Installation & Usage
1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/ETL-Liga1-Players.git
   ```
2. Open and run the **R Markdown file** in RStudio.
3. Modify `config.R` to set **input/output paths** as needed.

## 🔧 Future Enhancements
- Expand dataset with **player performance statistics**.  
- Integrate API support for **live player updates**.  
- Automate ETL pipeline using **scheduled R scripts**.  

## 📌 Contact & Links
🔗 **GitHub Repository:** [https://github.com/Josshua-DSA/ETL---Project/edit/main/README.md]  
🔗 **LinkedIn Profile:** [(https://www.linkedin.com/in/joshua-remedial-syeba-0024a8326/?trk=opento_sprofile_topcard)]  

---
**🚀 Let's analyze Liga 1 player data and extract valuable insights!**
