
# 🏥 Medical Device Prediction Flow (Power Automate + Azure AutoML)

This Power Automate flow allows healthcare providers to **automatically predict medical device alerts or anomalies** using a deployed Azure AutoML model. When new patient/device data is added (e.g., via Excel, SharePoint, or IoT), it sends the data to AutoML, interprets the results, and takes action (like notifying doctors or updating logs).

---

## 🎯 Goal

- Automate medical alert predictions using Azure Machine Learning
- Trigger workflows like email alerts, Teams notifications, or SharePoint updates
- Integrate seamlessly with Microsoft 365 tools (Excel, SharePoint, Forms)

---

## 🧱 Prerequisites

- ✅ Microsoft Azure Subscription
- ✅ Azure Machine Learning Workspace with deployed AutoML model (real-time endpoint)
- ✅ Power Automate (MS365) account
- ✅ Input data source: Excel, SharePoint List, Microsoft Forms, or IoT hub

---

## ⚙️ Flow Architecture

1. **Trigger**: A new data entry (e.g., Excel or SharePoint)
2. **Transform**: Compose block formats the input JSON
3. **Predict**: HTTP POST to Azure AutoML endpoint
4. **Parse**: Extract "Scored Labels" from the response
5. **Act**:
   - If anomaly: Send alert, log to SharePoint, notify Teams
   - Else: Log as normal record

---

## 🪜 Step-by-Step Setup

### 🔹 Step 1: Train and Deploy AutoML Model (Azure)

1. Go to [Azure ML Studio](https://ml.azure.com)
2. Create new AutoML experiment
   - Upload or select dataset
   - Define `Alert_Flag` or similar as the target column
   - Choose classification/regression
3. Deploy the best model as a **real-time endpoint**
   - Note the **endpoint URL**, **input schema**, and **API key**

---

### 🔹 Step 2: Create Power Automate Flow

1. Visit [Power Automate](https://flow.microsoft.com)
2. Select **Create > Automated cloud flow**
3. Choose a trigger:
   - “When a new row is added” (Excel)
   - “When an item is created” (SharePoint)

---

### 🔹 Step 3: Format Data as JSON

Use a **Compose** or **Select** block to structure the input like so:

```json
{
  "Inputs": {
    "data": [
      {
        "Patient_ID": "P123",
        "Heart_Rate": 98,
        "Blood_Oxygen": 91
      }
    ]
  }
}
```

---

### 🔹 Step 4: Call AutoML API

Use **HTTP** action:
- Method: `POST`
- URL: Your AutoML endpoint
- Headers:
  - `Authorization`: `Bearer <your-api-key>`
  - `Content-Type`: `application/json`
- Body: Your JSON payload

---

### 🔹 Step 5: Parse JSON Response

Use **Parse JSON** to extract:
```json
{
  "Results": {
    "Scored Labels": 1
  }
}
```

---

### 🔹 Step 6: Add Actions

Use **Condition** block:
- If `Scored Labels` = 1:
  - ✅ Send Outlook email to clinician
  - ✅ Log to SharePoint “Medical Alerts”
  - ✅ Trigger Microsoft Teams notification
- Else:
  - Log as normal patient record

---

## 🧪 Example

| Column       | Value  |
|--------------|--------|
| Patient_ID   | P123   |
| Heart_Rate   | 110    |
| Blood_Oxygen | 89     |

Prediction: `"Scored Labels": 1` → Sends alert: “Patient P123 needs attention.”

---

## 📦 Included Files

| File                          | Description                                 |
|-------------------------------|---------------------------------------------|
| `sample_automl_input.json`    | Example input format for Azure AutoML       |
| `flow_structure.json`         | Structure of Power Automate steps           |
| `MedicalPredictionFlow_Importable.zip` | Flow logic packaged for reference |

---

## 🔐 Notes on Security & Compliance

- Ensure patient data is anonymized and compliant with **HIPAA / GDPR**
- Use **Azure Key Vault** to manage sensitive API tokens
- Use HTTPS endpoints only

---

## 💬 Need Help?

For customization, integration, or automation enhancements, feel free to reach out or fork this project on GitHub.
