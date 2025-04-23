# Multimodal-LLM-
############################################# Multinomial LLM to extract information ################################################
import os
import base64
import mimetypes
from openai import OpenAI

# Initialize client
client = OpenAI(
  base_url="https://openrouter.ai/api/v1",
  api_key="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",
)

def process_image(image_path):
    mime_type = mimetypes.guess_type(image_path)[0]
    with open(image_path, "rb") as image_file:
        base64_image = base64.b64encode(image_file.read()).decode('utf-8')
    data_url = f"data:{mime_type};base64,{base64_image}"
    
    completion = client.chat.completions.create(
        model="qwen/qwen2.5-vl-72b-instruct:free",
        messages=[
            {
                "role": "user",
                "content": [
                    {
                        "type": "text",
                        "text": "Analyze this medical document. Extract ALL text elements. Use this 
                        exact JSON format:\n{'text_boxes': [{'text': '...'}\nConsider faint text, stamps, and handwritten notes. Return ONLY the JSON."
                    },
                    {
                        "type": "image_url",
                        "image_url": {
                            "url": data_url
                        }
                    }
                ]
            }
        ]
    )
    
    return completion.choices[0].message.content

def main(): 
    input_folder = "data"  # Folder containing input images
    output_folder = "outputs"
    
    # Create output directory if not exists
    os.makedirs(output_folder, exist_ok=True)
    list = os.listdir(input_folder)
    # Process all images in the input folder
    for filename in list[1:4]: # Limit to first 4 images for testing
        if filename.lower().endswith(('.png', '.jpg', '.jpeg', '.tiff', '.bmp')):
            image_path = os.path.join(input_folder, filename)
            
            try:
                result = process_image(image_path)
                
                # Create output filename
                output_filename = os.path.splitext(filename)[0] + ".txt"
                output_path = os.path.join(output_folder, output_filename)
                
                # Save result
                with open(output_path, "w") as f:
                    f.write(result)
                
                print(f"Processed {filename} successfully")
            
            except Exception as e:
                print(f"Error processing {filename}: {e}")

if __name__ == "__main__":
    main()

############################# output ##########################
# 1.jpg
```json
{
  "text_boxes": [
    {
      "text": "Dr B. Who\nFarmstreet 12\nKirkville\ntel. 3876"
    },
    {
      "text": "R/\ndate 1 nov 1994"
    },
    {
      "text": "Dioxin 0.125 mg\ntablet da no.7\n$ 1 dd 1 tablet"
    },
    {
      "text": "B. WHO"
    },
    {
      "text": "Ms/Mr Patient 30\naddress:\nage: 70 years"
    }
  ]
}
```
#10.jpg
```json
{
  "text_boxes": [
    {
      "text": "Dr. Naveen Polavarapu\nMRCP (UK), FRCP (Gastro), CCT (Gastro), Liver Transplant Fellowship\nConsultant Gastroenterologist and Transplant Hepatologist\nRegd.No. 46206\nP. 040-2360 7777, Extn 4005/1142\nFor appointments call between 10 am - 6 pm : 7382778899\nEmail : docpolav@gmail.com\n84/8/21\nTO WHOM SO EVER IT MAY CONCERN\nThis is to inform that Mr. CH. SAMUEL 62yr Male\nis currently admitted under my care with IP.No: 367588.\nHe is currently undergoing treatment on ICU for\nsevere sepsis with MODS (likely liver abscess with\naspiration pneumonia). He further needs hospital stay\nfor about 8-10 days for complete recovery. This is to\ninform and kindly do the needful.\nThanking You.\nDr. NAVEEN POLAVARAPU\nMRCP (Lon), MRCP (Edin), MRCP (Gastro), CCT (Gastro)\nConsultant\nTransplant Hepatologist\nRegd. No:46206\nApollo Hospitals, Jubilee Hills, Hyd-56.\nOPPO A52\nApollo Health City Campus, Jubilee Hills, Hyderabad - 500 096, India. +91-1860 258 1066 Fax: +91-40-23608050, Emergency Call-1066\napollohealthcity@apollohospitals.com www.apollohealthcity.com apollohealthcity apollohealthhyd apollohealthcityhyd"
    }
  ]
}
```

#########################  Graphical represntation in R  ############################################################
 
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

################################################################################################# Empirical likelihood based inference in R (Related to my phd research) #####################################################################################

library(emplik)

# Subset of data extracted from MLLM
dosage_data <- c(300, 400, 500, 450, 375)  # simulated Ibuprofen dosages
# Suppose we want to test whether the average dosage of Ibuprofen equals 400 mg.
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
