# assisitants-api

from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()
client = OpenAI()

# create assistant or use exisiting assistant id 
assistant_id = "asst_hQUAeMD9gVrxzoby0bX35XkF"
# create thread
thread = client.beta.threads.create()

# add messages to the thread

message =client.beta.threads.messages.create(
    thread_id=thread.id,
    role="user",
    content="solve 1 + 1 ?"
    
)

run = client.beta.threads.runs.create(
    thread_id=thread.id,
    assistant_id=assistant_id,
)

while run.status !="completed":
    run = client.beta.threads.runs.retrieve(
        thread_id=thread.id,
        run_id=run.id
        )
    print(f"run status : {run.status}")
else :
    print("run completed")


messages = client.beta.threads.messages.list(
  thread_id=thread.id
)

for message in messages:
    print(f"role : {message.role} \n content : {message.content[0].text.value}")
