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
import shutil
from apiclient import errors
from email.mime.text import MIMEText
from google_auth_oauthlib.flow import InstalledAppFlow
from google.auth.transport.requests import Request
from googleapiclient.discovery import build

scope = ["https://www.googleapis.com/auth/gmail.modify"]

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
  for label in labels:
    print(f"[get_labels] name: {label['name']}")
    print(f"[get_labels]   id  : {label['id']}")
    print(f"[get_labels]   type: {label['type']}")
  return labels

def set_read(service, user_id, message_ids, unread_id):
  try:
    service.users().messages().batchModify(userId=user_id, body={
      'ids'           : message_ids,
      'removeLabelIds': unread_id,
      'addLabelIds'   : []
    }).execute()
  except errors.HttpError as error:
    print(f"[get_mails] Network Error Occurred: {error}")

def get_mails(service, user_id, query, unread_id, limit):
  try:
    messages = service.users().messages().list(userId=user_id, q=query, labelIds=unread_id, maxResults=limit).execute()
    if messages['resultSizeEstimate'] == 0:
      print(f"Search Results is None")
      return 0
    message_ids = [message['id'] for message in messages['messages']]
    for message_id in message_ids:
      data = service.users().messages().get(userId=user_id, id=message_id).execute()
      headers = data['payload']['headers']
      for header in headers:
        header_name_log = f"[get_mails] header_name: {header['name']}"
        print(header_name_log[:shutil.get_terminal_size().columns])
        header_value_log = f"[get_mails]   header_value: {header['value']}"
        print(header_value_log[:shutil.get_terminal_size().columns])
      bodies = []
      if 'data' in data['payload']['body']:
        body = [base64.urlsafe_b64decode(data['payload']['body']['data']).decode("UTF-8")]
        bodies.append(body)
        body_log = f"[get_mails] body: {body}"
        print(body_log[:shutil.get_terminal_size().columns])
      else:
        for part in data['payload']['parts']:
          if part['mimeType'] == "text/plain":
            body = base64.urlsafe_b64decode(part['body']['data']).decode("UTF-8")
            bodies.append(body)
            body_log = f"[get_mails] body: {body}"
            print(body_log[:shutil.get_terminal_size().columns])
    set_read(service, user_id, message_ids, unread_id)
  except errors.HttpError as error:
    print(f"[get_mails] Network Error Occurred: {error}")

def main():
  query = 'no-reply'
  credential = get_credential()
  service = build("gmail", "v1", credentials=credential, cache_discovery=False)
  unread_id = [label['id'] for label in get_labels(service, "me") if label['name'] == "UNREAD"]
  get_mails(service, "me", query, unread_id, limit=100)

if __name__ == "__main__": main()
```

> ## References
> - https://qiita.com/muuuuuwa/items/822c6cffedb9b3c27e21
