# Experiment – 6: Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights. 

### Register No.: 212222083008  
### Date: 30.09.2025

## Aim:
Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights.

## AI Tools Required:
•	Text Interpretation & Analysis: OpenAI GPT-4, Claude
•	Image Captioning & Scene Detection: DeepAI API
•	Optional Visualization/Output: Matplotlib, JSON

## Use Case:
To develop a system that captures and analyzes traffic images from urban environments and identifies traffic congestion levels using AI tools. It will:
•	Detect the number and type of vehicles in a traffic image
•	Use GPT to interpret the scene and give a congestion score
•	Compare outputs from multiple tools (DeepAI and OpenAI)
•	Generate actionable traffic-related insights

## Objective:
To create a Python-based system that:
•	Fetches images from an input dataset (traffic photos)
•	Uses DeepAI API for object detection (vehicle counting)
•	Sends detected data to GPT-4 for interpretation
•	Compares the model outputs
•	Produces human-readable insights (e.g., congestion score, suggestions)

## Procedure / Algorithm:

### Step 1: Define the Use Case
Traffic congestion analysis using image input.
The application processes traffic images to detect how crowded the roads are and suggests insights like rerouting or alerts.

### Step 2: Set Up API Access
Use environment variables to securely store API keys:
import os
OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
DEEPAI_API_KEY = os.getenv("DEEPAI_API_KEY")

### Step 3: DeepAI Vehicle Detection Function
import requests

def detect_vehicles(image_path):
    url = "https://api.deepai.org/api/densecap"
    with open(image_path, 'rb') as img_file:
        response = requests.post(
            url,
            files={'image': img_file},
            headers={'api-key': DEEPAI_API_KEY}
        )
    return response.json()

### Step 4: OpenAI GPT Interpretation Function
import openai

def interpret_traffic_scene(detection_results):
    openai.api_key = OPENAI_API_KEY
    prompt = f"Based on the following scene description: {detection_results}, evaluate the level of traffic congestion (low, medium, high) and suggest any necessary action."
    
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    return response['choices'][0]['message']['content']

### Step 5: Run Test Cases (Image Dataset)
test_images = ["traffic_1.jpg", "traffic_2.jpg", "traffic_3.jpg"]
results = []

for img in test_images:
    detection_data = detect_vehicles(img)
    gpt_analysis = interpret_traffic_scene(detection_data)
    results.append({
        "image": img,
        "detections": detection_data,
        "gpt_summary": gpt_analysis
    })

### Step 6: Generate Actionable Insights
def generate_traffic_insights(summary):
    if "high" in summary.lower():
        level = "High Congestion"
        action = "Trigger alert to reroute vehicles."
    elif "medium" in summary.lower():
        level = "Moderate Congestion"
        action = "Consider dynamic signal timing."
    else:
        level = "Low Congestion"
        action = "No action needed."
    return {"level": level, "suggested_action": action}

insights = [generate_traffic_insights(res['gpt_summary']) for res in results]

### Step 7: Save Insights to File
import json

with open("traffic_congestion_report.json", "w") as file:
    json.dump(insights, file, indent=4)

## Conclusion:
This experiment demonstrates the integration of multiple AI services in Python to create a traffic congestion analysis tool. DeepAI's object detection provided the raw data (vehicle descriptions), while GPT-4 interpreted the data to assess congestion and suggest actionable traffic strategies. The code is modular and scalable to other domains such as parking monitoring or road hazard detection.

# Result:
The Python script successfully integrated DeepAI and OpenAI GPT-4 to analyze traffic scenes. It was able to:
•	Identify and describe vehicles in urban images
•	Interpret the congestion level
•	Suggest actions based on AI analysis
•	Save all results for reporting and further improvement
Thus, the corresponding prompt was executed successfully.



