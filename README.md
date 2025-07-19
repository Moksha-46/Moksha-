import openai
import time
import os

# Set your OpenAI API key
openai.api_key = "YOUR_OPENAI_API_KEY"

# Settings
MODEL = "gpt-3.5-turbo"  # or "gpt-4"
TEMPERATURE = 0.7
MAX_TOKENS = 1000
SYSTEM_PROMPT = "You are an assistant that helps with creative text generation tasks."

# Store conversation history
conversation_history = [{"role": "system", "content": SYSTEM_PROMPT}]

def generate_text(user_input):
    """Send a prompt to ChatGPT and return the response."""
    # Append user input
    conversation_history.append({"role": "user", "content": user_input})

    try:
        response = openai.ChatCompletion.create(
            model=MODEL,
            messages=conversation_history,
            temperature=TEMPERATURE,
            max_tokens=MAX_TOKENS,
            n=1,
            stop=None
        )

        reply = response['choices'][0]['message']['content']
        # Append assistant's reply to history
        conversation_history.append({"role": "assistant", "content": reply})
        return reply

    except openai.error.OpenAIError as e:
        return f"Error: {e}"

def clear_console():
    os.system('cls' if os.name == 'nt' else 'clear')

def main():
    clear_console()
    print("🧠 ChatGPT Text Generator - Powered by OpenAI API")
    print("Type 'exit' or 'quit' to stop.")
    print("------------------------------------------------\n")

    while True:
        user_input = input("👤 You: ")
        if user_input.lower() in ['exit', 'quit']:
            print("Exiting...")
            break

        print("🤖 ChatGPT is thinking...")
        response = generate_text(user_input)
        print(f"🤖 ChatGPT: {response}\n")
        time.sleep(1)

if __name__ == "__main__":
    main()
