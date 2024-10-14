# Calculate_AWS_Daily_Cost
## For Centos
````sudo yum update -y
sudo yum install -y python3
python3 --version
yum install -y python3-pip
pip3 --version
````
## For Ubuntu
````sudo apt update -y
sudo apt upgrade -y
sudo apt install -y python3
python3 --version
sudo apt install -y python3-pip
pip3 --version
````

![image](https://github.com/user-attachments/assets/8239b087-964f-4cc1-bd5b-ae8d8cd510d5)
## Install AWS CLI
```
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
````
![image](https://github.com/user-attachments/assets/0894ad58-8287-4e08-afc4-1d0ad810a120)
## Create AWS Profile
````
aws configure --profile=shruti
access_key_id=
secret_key_id=
````
![image](https://github.com/user-attachments/assets/8d1be0c9-120b-4fd9-83c6-dedc5aa3a965)
## Create Directory "AWS_Cost"
````
mkdir AWS_Script
````
![image](https://github.com/user-attachments/assets/8cc51ff4-c510-4fe9-a2f4-c3dc2d225320)

## Give Permissions to file and directory
## Bash_Script
````
sudo vim aws_cost_daily.sh     ----> (make necessary changes in file)
````
![image](https://github.com/user-attachments/assets/d7fbb6e5-9c53-4933-85fa-7116d074da63)
````
#!/bin/bash

# Variables
TODAY=$(date +"%Y-%m-%d")
YESTERDAY=$(date -d "yesterday" +"%Y-%m-%d")
PROFILE="abhipray"  # Replace with your AWS CLI profile name if needed

# Fetch costs
aws ce get-cost-and-usage \
    --time-period Start=$YESTERDAY,End=$TODAY \
    --granularity DAILY \
    --metrics "BlendedCost" \
    --query 'ResultsByTime[0].Total.BlendedCost.Amount' \
    --profile $PROFILE \
    --output text
                
````

## Run Bash Script
````
chmod +x aws_cost_daily.sh
./aws_cost_daily.sh
````
## Python_Script
````
sudo vim aws_daily_cost.py
````
![image](https://github.com/user-attachments/assets/ea97eb71-d3b2-4bda-8aa0-0ce8df86456d) 
###The above image shows error..

````
import boto3
import datetime
 
def get_aws_daily_cost():
    # Initialize AWS Cost Explorer

   session = boto3.Session(profile_name='shruti')
    client = boto3.client('ce', region_name='us-east-1')
 
    # Get yesterday's date
    end = datetime.datetime.utcnow().date()
    start = end - datetime.timedelta(days=1)
 
    # Retrieve cost data
    response = client.get_cost_and_usage(
        TimePeriod={
            'Start': start.strftime('%Y-%m-%d'),
            'End': end.strftime('%Y-%m-%d')
        },
        Granularity='DAILY',
        Metrics=['BlendedCost']
    )
 
    # Extract and return the cost
    return float(response['ResultsByTime'][0]['Total']['BlendedCost']['Amount'])
 
def save_cost_to_file(cost):
    with open('daily_cost.txt', 'a') as f:
        f.write(f"{datetime.datetime.utcnow().strftime('%Y-%m-%d')}: ${cost}\n")
 
if __name__ == "__main__":
    daily_cost = get_aws_daily_cost()
    print(f"Daily AWS Account Cost: ${daily_cost}")
    save_cost_to_file(daily_cost)
````
![image](https://github.com/user-attachments/assets/6644a950-a0e9-4399-9efc-15bbdc480c26)

## Run Python Script
````
python3 aws_daily_cost.py
````
![image](https://github.com/user-attachments/assets/842971e2-0278-418f-9186-6d142531b674)








