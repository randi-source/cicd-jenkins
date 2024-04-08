# Setup Virtual Environment

```python
conda create -n jenkins-env python=3.10 -y
conda activate jenkins-env
pip install -r requirements.txt
pip install .
```

# Test the FASTAPI

```json
{
  "Gender": "Male",
  "Married": "No",
  "Dependents": "2",
  "Education": "Graduate",
  "Self_Employed": "No",
  "ApplicantIncome": 5849,
  "CoapplicantIncome": 0,
  "LoanAmount": 1000,
  "Loan_Amount_Term": 1,
  "Credit_History": "1.0",
  "Property_Area": "Rural"
}
```

# Docker Commands

```
docker build -t loan_pred:v1 .
docker build -t randies/cicd:latest .
docker push randies/cicd:latest

docker run -d -it --name modelv1 -p 8005:8005 randies/cicd:latest bash

docker exec modelv1 python prediction_model/training_pipeline.py

docker exec modelv1 pytest -v --junitxml TestResults.xml --cache-clear

docker cp modelv1:/code/src/TestResults.xml .

docker exec -d -w /code modelv1 python main.py

docker exec -d -w /code modelv1 uvicorn main:app --proxy-headers --host 0.0.0.0 --port 8005

var=$(docker ps --format "{{.Names}}" | head -n 1)

```