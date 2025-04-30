# 🚀 n8n Workflow: If Gate Example

This guide walks you through creating, importing, and running a simple **n8n workflow** that checks if a person's age is 18 or older. It includes the full JSON code, step-by-step instructions, and emojis for clarity! 🎉

---

## 🌟 What the Workflow Does

1. **Start Manually** 🏁 — Begins with a Webhook trigger.
2. **Set a Value** 📝 — Sets `age = 20`.
3. **IF Condition** 🔍 — Checks if age is >= 18.
4. **Path Selection** 🛤️ — Chooses either:
   - **Yes Path** ✅: age >= 18
   - **No Path** 🚫: age < 18

---

## 📋 Workflow JSON Code
Save the following code into a file named `if-gate-workflow.json`:

````json
[
  {
    "name": "If Gate Example",
    "nodes": [
      {
        "parameters": {
          "path": "if-gate-example",
          "responseMode": "onReceived",
          "options": {}
        },
        "name": "Start",
        "type": "n8n-nodes-base.webhook",
        "typeVersion": 1,
        "position": [200, 300],
        "webhookId": "if-gate-example"
      },
      {
        "parameters": {
          "values": {
            "string": [],
            "number": [
              {
                "name": "age",
                "value": 20
              }
            ]
          },
          "options": {}
        },
        "name": "Set Data",
        "type": "n8n-nodes-base.set",
        "typeVersion": 1,
        "position": [400, 300]
      },
      {
        "parameters": {
          "conditions": {
            "number": [
              {
                "value1": "={{$json[\"age\"]}}",
                "operation": "largerEqual",
                "value2": 18
              }
            ]
          }
        },
        "name": "Check Age",
        "type": "n8n-nodes-base.if",
        "typeVersion": 1,
        "position": [600, 300]
      },
      {
        "parameters": {
          "functionCode": "return [{ json: { result: 'YES: Age is 18 or older' } }];"
        },
        "name": "Yes Path",
        "type": "n8n-nodes-base.function",
        "typeVersion": 1,
        "position": [800, 200]
      },
      {
        "parameters": {
          "functionCode": "return [{ json: { result: 'NO: Underage' } }];"
        },
        "name": "No Path",
        "type": "n8n-nodes-base.function",
        "typeVersion": 1,
        "position": [800, 400]
      }
    ],
    "connections": {
      "Start": {
        "main": [[{ "node": "Set Data", "type": "main", "index": 0 }]]
      },
      "Set Data": {
        "main": [[{ "node": "Check Age", "type": "main", "index": 0 }]]
      },
      "Check Age": {
        "main": [
          [{ "node": "Yes Path", "type": "main", "index": 0 }],
          [{ "node": "No Path", "type": "main", "index": 0 }]
        ]
      }
    },
    "active": true
  }
]
````

---

## 🛠️ Step-by-Step Instructions

### 📄 Step 1: Create the JSON File

- **Windows**:
  ```bash
  notepad if-gate-workflow.json
  ```
- **macOS/Linux**:
  ```bash
  nano if-gate-workflow.json
  ```

📌 Paste the JSON code and save the file.

---

### 📥 Step 2: Import the Workflow into n8n

1. Install n8n if needed:
   ```bash
   npm install -g n8n
   ```
2. Start n8n:
   ```bash
   n8n start
   ```
3. In a new terminal, import the workflow:
   ```bash
   n8n import:workflow --input=if-gate-workflow.json
   ```

---

### 🖥️ Step 3: Run the Workflow in Web UI

1. Open: [http://localhost:5678](http://localhost:5678)
2. Find **If Gate Example** in the sidebar.
3. Ensure it's **active** (toggle switch should be green).
4. Copy the webhook URL (e.g., `http://localhost:5678/webhook/if-gate-example`).

Trigger the workflow:
```bash
curl -X POST http://localhost:5678/webhook/if-gate-example
```
*Or click the **Execute Workflow** button (▶️) in the UI.*

---

### 🔍 Step 4: Check the Output

After execution:
- Go to the **Executions** tab.
- Click the latest entry.
- You’ll see:
  - ✅ YES path if `age >= 18` → `YES: Age is 18 or older`
  - 🚫 NO path if `age < 18` → `NO: Underage`

Sample Output:
```json
[{ "json": { "result": "YES: Age is 18 or older" } }]
```
or
```json
[{ "json": { "result": "NO: Underage" } }]
```

---

## 🧪 Test with a Different Age

1. Edit **Set Data** node:
   - Change age to 15.
2. Save the workflow.
3. Trigger again.
4. Output should be:
```json
NO: Underage
```

---

## 🎈 Final Notes

- You can trigger the webhook programmatically from a Node.js app using `axios` or `fetch`.
- Ensure n8n is **running** during tests.
- Double-check:
  - Workflow is **active**.
  - Webhook URL is **correct**.

Need help? Just ask! 😊

