# Multimodal-LLM-
######################Python Codes################
import pytesseract
from PIL import Image

def extract_text_from_image(image_path):
    # Open the image file
    img = Image.open("106.jpg")

    # Use Tesseract to do OCR on the image
    text = pytesseract.image_to_string(img)

    return text


image_path = '106.jpg'  # Provide the path to your image
text = extract_text_from_image(image_path)

print("Extracted Text:")
print(text)

# Clone repo
git clone https://github.com/clovaai/donut.git
cd donut

# Set up environment
conda create -n donut python=3.8
conda activate donut
pip install -r requirements.txt

# Download pretrained model (document parsing)
python app.py --image_path /path/to/prescription1.png
{
  "patient_name": "John Doe",
  "doctor": "Dr. Patel",
  "date": "2025-04-20",
  "medications": [
    {"name": "Ibuprofen", "dosage": 400},
    {"name": "Amoxicillin", "dosage": 250}
  ]
}
#######################Graphical represntation in R ########
# Summary of medications
summary_stats <- data %>%
  group_by(medication) %>%
  summarise(
    n = n(),
    avg_dose = mean(dosage),
    sd_dose = sd(dosage)
  )
print(summary_stats)

# Visualize distribution
ggplot(data, aes(x = medication, y = dosage, fill = medication)) +
  geom_boxplot() +
  labs(title = "Dosage Distribution by Medication", y = "Dosage (mg)", x = "") +
  theme_minimal()

#################### Empirical likelihood based inference in R #################

library(emplik)

# Subset of data extracted from MLLM
dosage_data <- c(300, 400, 500, 450, 375)  # simulated Ibuprofen dosages

# Empirical likelihood test
el_test <- el.test(dosage_data, mu = 400)
print(el_test)

########## Output ##
$`-2LLR`
[1] 0.02697791

$Pval
[1] 0.8695347

$lambda
[1] 0.00107533

$grad
[1] 3.541611e-13

$hess
         [,1]
[1,] 23618.79

$wts
[1] 1.1204896 1.0000000 0.9029076 0.9489768 1.0276259

$nits
[1] 4
