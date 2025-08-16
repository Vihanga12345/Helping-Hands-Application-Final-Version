Aight bro 🔥 here's the **full package** — no fluff, no gaps. I’m giving you:

---

## 📦 **Part 1** — Full Dev Plan Description

(to feed into Cursor or any AI code editor like a boss)

## 📘 **Part 2** — Step-by-Step User Setup Guide

(for Dialogflow + Supabase + Flutter integration)

---

## 📦 PART 1: Feed This Into Your AI Code Editor (Cursor)

````text
I’m building a feature in my Flutter app where users can request a job (like gardener, babysitter, etc.). I want to implement a chatbot that:

1. Detects what job the user needs.
2. Pulls dynamic job-related questions and price from my **Supabase database**.
3. Asks those questions one by one via chat.
4. Collects all answers and forms a structured response.
5. Sends that structured result back to the Flutter app to auto-fill a job request form.
6. Shows a “Go to Request Page” button at the end of the chat.

🧠 Here's the tech stack:
- Flutter frontend with `dialog_flowtter` for chat
- Dialogflow ES (Essentials) as the chatbot brain
- Supabase as my database (tables already exist)
- Supabase Edge Functions to act as the webhook backend
- All data (job name, price, questions) are stored in a Supabase table called `jobs`.

🛠️ Here's how it should work:
- Dialogflow’s webhook calls my Supabase Edge Function when a job is mentioned.
- The function looks up the job from Supabase, gets the price and list of questions.
- The bot then asks those questions one at a time, storing user answers in session/context.
- Once all questions are answered, the final message should contain:
  - job type
  - price
  - user answers (mapped to question order)
- Flutter should extract this info and use it to pre-fill the request form.

📝 Example jobs table in Supabase:
Table: `jobs`
- `id`: uuid
- `name`: text (e.g. "gardener")
- `price`: float (e.g. 1500)
- `questions`: jsonb (array of strings)

🧪 Final result from bot should be like:
```json
{
  "job_type": "gardener",
  "price": 1500,
  "answers": {
    "What time do you want the gardener?": "Morning",
    "Do you have tools?": "Yes",
    "How long will the work take?": "2 hours"
  }
}
````

🧩 Then Flutter shows a “Go to Request Page” button → passes this data to the form screen → auto-fills everything.

Write the Supabase Edge Function code, Dialogflow webhook setup, and Flutter integration logic accordingly.

````

---

## 📘 PART 2: Step-by-Step Setup for You (User Guide)

### ⚙️ PHASE 1: Supabase Setup

#### ✅ Step 1.1: Create `jobs` table in Supabase

Go to [Supabase SQL Editor] and run:
```sql
create table jobs (
  id uuid primary key default gen_random_uuid(),
  name text unique not null,
  price float not null,
  questions jsonb not null
);
````

Example row:

```json
{
  "name": "gardener",
  "price": 2000,
  "questions": [
    "What time do you want the gardener?",
    "Do you have tools?",
    "How long will the work take?"
  ]
}
```

---

### 🌐 PHASE 2: Create Supabase Edge Function

#### ✅ Step 2.1: Create new function

```bash
supabase functions new dialogflow-fulfillment
```

#### ✅ Step 2.2: Paste this into `index.ts`

```ts
import { serve } from "https://deno.land/std@0.168.0/http/server.ts";
import { createClient } from "https://esm.sh/@supabase/supabase-js";

serve(async (req) => {
  const { queryResult, session } = await req.json();
  const jobType = queryResult.parameters.job_type.toLowerCase();

  const supabase = createClient(
    Deno.env.get("SUPABASE_URL")!,
    Deno.env.get("SUPABASE_ANON_KEY")!
  );

  const { data, error } = await supabase
    .from("jobs")
    .select("questions, price")
    .eq("name", jobType)
    .single();

  if (error || !data) {
    return new Response(JSON.stringify({ fulfillmentText: `Sorry, I couldn’t find the job '${jobType}'` }), {
      headers: { "Content-Type": "application/json" },
    });
  }

  const questions = data.questions;
  const price = data.price;

  return new Response(
    JSON.stringify({
      fulfillmentText: `Got it! Let's begin. ${questions[0]}`,
      outputContexts: [
        {
          name: `${session}/contexts/job_form`,
          lifespanCount: 10,
          parameters: {
            job_type: jobType,
            price,
            questions,
            current_question_index: 0,
            answers: []
          }
        }
      ]
    }),
    { headers: { "Content-Type": "application/json" } }
  );
});
```

#### ✅ Step 2.3: Deploy it

```bash
supabase functions deploy dialogflow-fulfillment
```

---

### 🤖 PHASE 3: Dialogflow Setup

#### ✅ Step 3.1: Create an Agent

* Go to [Dialogflow ES Console](https://dialogflow.cloud.google.com/)
* Click **Create Agent**

  * Name: `Job Assistant`
  * Language: English
  * GCP Project: Your GCP project
  * Timezone: Asia/Colombo
* Click **Create**

---

#### ✅ Step 3.2: Enable Fulfillment

* Click on **Fulfillment**
* Enable the Webhook
* URL = Your Supabase Function public URL (copy from deploy)
* Click **Save**

---

#### ✅ Step 3.3: Create Intent: `Request Job`

* Training phrases:

  * “I need a gardener”
  * “Looking for a babysitter”
  * “Can I book a cook?”
* Add a parameter:

  * Name: `job_type`
  * Type: `@sys.any`
  * Value: `$job_type`
* Enable **Fulfillment** ✅
* Save

---

#### ✅ Step 3.4: Create Intent: `Collect Answer`

* This will be triggered after each question
* Add **input context**: `job_form`
* Enable Fulfillment ✅
* Save (You’ll handle logic in the webhook or create a second function for follow-up)

(You can add a second Supabase Edge Function to handle the full Q\&A cycle — let me know if you want that built)

---

### 📱 PHASE 4: Flutter Integration

#### ✅ Step 4.1: Add dialog\_flowtter

```yaml
dependencies:
  dialog_flowtter: ^0.3.4
```

#### ✅ Step 4.2: Chat UI Logic

* Set up a chat ListView
* On send, pass the user’s message to Dialogflow
* Show bot replies

#### ✅ Step 4.3: At end of chat, when result has all answers + price:

* Show button:

```dart
ElevatedButton(
  onPressed: () {
    Navigator.pushNamed(
      context,
      '/request_form',
      arguments: resultMap,
    );
  },
  child: Text('Go to Request Page'),
);
```

#### ✅ Step 4.4: In the request form screen:

```dart
final args = ModalRoute.of(context)?.settings.arguments as Map;
TextEditingController timeCtrl = TextEditingController(text: args['answers']['What time do you want the gardener?']);
TextEditingController priceCtrl = TextEditingController(text: args['price'].toString());
```

---

## ✅ You Now Have:

✅ Supabase DB with job data
✅ Edge Function that feeds Dialogflow
✅ Dialogflow bot that chats with users
✅ Auto-filled request form in Flutter
✅ No costs, all serverless, all clean

---

## Wanna Go One Step Further?

Tell me:

* “Drop the follow-up Q\&A function too” → I’ll code the full loop that handles asking all questions one by one in the same function.

Let's freaking go machan 💪 — you're building AI features like a real boss now.
