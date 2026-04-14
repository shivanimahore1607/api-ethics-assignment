# api-ethics-assignment

## Task 1 — Classify and Handle PII Fields

full_name → Direct PII → Pseudonymize (replace with user ID)  
Reason: Ye directly person ko identify karta hai.

email → Direct PII → Drop / Hash (Pseudonymize)  
Reason: Email se direct identity aur contact possible hai.

date_of_birth → Direct PII → Generalize (convert to age group)  
Reason: DOB se person identify ho sakta hai.

zip_code → Indirect PII → Generalize (use partial zip code)  
Reason: Location se re-identification possible hai.

job_title → Indirect PII → Keep / Generalize  
Reason: Alone identify nahi karta but combine ho sakta hai.

diagnosis_notes → Sensitive data → Remove / Mask  
Reason: Medical information very sensitive hoti hai.

---
# Task 2 - Audit the API Script for Ethical Compliannce

import requests
import os
import time

# API configuration
API_URL = "https://healthstats-api.example.com/records"
API_KEY = os.getenv("API_KEY")

# Safety check for API key
if not API_KEY:
    raise Exception("API key not found. Please set API_KEY in environment variables.")

records = []

# Data collection with limits and safety practices
MAX_PAGES = 10  # data minimization
DELAY = 1  # rate limiting to avoid API abuse

for page in range(1, MAX_PAGES + 1):
    try:
        response = requests.get(
            API_URL,
            params={
                "page": page,
                "key": API_KEY
            },
            timeout=10
        )

        # Check response status
        if response.status_code == 200:
            data = response.json()

            # Ensure results exist before adding
            if "results" in data:
                records.extend(data["results"])
        else:
            print(f"Error fetching page {page}: {response.status_code}")

    except requests.exceptions.RequestException as e:
        print(f"Request failed on page {page}: {e}")

    time.sleep(DELAY)

# Store cleaned records
def save_to_database(data):
    print(f"Saving {len(data)} records safely to database...")
    # database logic here

save_to_database(records)