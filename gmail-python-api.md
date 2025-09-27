# Python API of Gmail

## 1. Access to the `Google Cloud Platform`
- https://console.cloud.google.com/welcome/new

## 2. Create New Project

## 3. Enable `Gmail API`
- https://console.cloud.google.com/marketplace/product/google/gmail.googleapis.com

## 4. Open `OAuth consent screen`
- https://console.cloud.google.com/auth/overview

## 5. Open `Get Started` to Create New Project
- App Information:
  - gmail-python-api
- Audience:
  - External

## 6. Open `Data Access` to Add Scope
- `Add or remove scopes`
  - Enable `../auth/gmail.modify`

## 7. Open `OAuth Clients`
- https://console.cloud.google.com/auth/clients

- Open `Create client`
  - Application type
    - Desktop app
  - Name:
    - python-app
- Download JSON

## 8. Add Test Users
- https://console.cloud.google.com/auth/audience

- `Test users` > `+ Add users`
  - Add self gmail address

## 9. Install Required Packages
```
python3 -m pip install oauth2-client google-api-python-client google-auth-httplib2 google-auth-oauthlib --break-system-packages
```

## 10. Python Program
```python
import base64
import csv
import io
import os
import pickle
from apiclient import errors
from email.mime.text import MIMEText
from google_auth_oauthlib.flow import InstalledAppFlow
from google.auth.transport.requests import Request
from googleapiclient.discovery import build

scope = "https://www.googleapis.com/auth/gmail.modify"

def get_credential():
  credential = None
  if os.path.exists("token.pickle"):
    with open("token.pickle", "rb") as token:
      credential = pickle.load(token)
  if (not credential) or (not credential.valid):
    if credential and credential.expired and credential.refresh_token:
      credential.refresh(Request())
    else:
      flow = InstalledAppFlow.from_client_secrets_file("client_id.json", scope)
      credential = flow.run_local_server(open_browser=False)
    with open("token.pickle", "wb") as token:
      pickle.dump(credential, token)
  return credential

def get_labels(service, user_id):
  response = service.users().labels().list(userId=user_id).execute()
  labels = response['labels']
  print(f"[get_labels] labels = {labels}")
  return labels

def get_mails(service, user_id, query, label_id, limit):
  try:
    messages = service.users().messages().list(userId=user_id, q=query, labelIds=[label_id], maxResults=limit).execute()
    if messages['resultSizeEstimate'] == 0:
      print(f"Search Results is None")
      return 0
    for message in messages['messages']:
      data = service.users().get(userId=user_id, id=message['id']).execute()
      headers = data['payload']['headers']
      for header in headers:
        print(f"[get_mails] header_name: {header['name']}, header_value: {header['value']}")
      bodies = []
      if 'data' in data['payload']['body']:
        body = [decode_base64url_data(data['payload']['body']['data'])]
        bodies.append(body)
        print(f"[get_mails] body: {body}")
      else:
        for part in data['payload']['parts']:
          if part['mimeType'] == "text/plain":
            body = decode_base64url_data(part['body']['data'])
            bodies.append(body)
            print(f"[get_mails] body: {body}")
    return [headers, bodies]
  except errors.HttpError as error:
    print(f"[get_mails] Network Error Occurred: {error}")
  return [None, None]

def main():
  query = 'no-reply'
  credential = get_credential()
  service = build("gmail", "v1", credentials=[credential], cache_discovery=False)
  label_ids = [label['id'] for label in get_labels(service, "me")]
  headers, bodies = get_mails(service, "me", query, label_ids, limit=10)
  return False

if __name__ == "__main__": main()
```

> ## References
> - https://qiita.com/muuuuuwa/items/822c6cffedb9b3c27e21
