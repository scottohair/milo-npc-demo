
# AI Chatbot NPC API Demonstration

![AI Chatbot](https://github.com/scottohair/milo-npc-demo/blob/main/chatbot.png)

## Overview

This repository demonstrates the capabilities of AI chatbots by showcasing NPC (Non-Playable Character) interaction in video games. The API allows developers to create rich, interactive NPCs with advanced conversational abilities powered by OpenAI's language models.

## API Description

The API is designed to facilitate the creation and management of AI-driven NPCs in video games. Each NPC is defined with a unique set of characteristics, dialogues, and actions that enhance player interaction.

### JSON Object Structure

Each NPC is defined using a JSON structure that includes headers for metadata, body for NPC details, dialogues for interactive messages, actions for possible NPC actions, and custom fields for additional information.

```json
{
  "header": {
    "version": "1.0",
    "timestamp": "2024-06-22T10:30:00Z",
    "message_type": "npc_creation",
    "authorization": "Bearer your_api_token_here"
  },
  "body": {
    "npc_id": "0000120210",
    "chat_context_id": "023819892410",
    "name": "Milo The Light",
    "initial_response": "Hi, I am Milo The Light! How can I help you?",
    "description": "A tech genius and visionary inspired by the moon's energy, constantly developing cutting-edge technologies. Known for discovering and managing artists like Tekashi 6ix9ine and Trippie Redd, and pioneering in the NFT space. Collaborates with Create Music Group and Bored Ape 'Jimbo' for innovative projects. A beacon of change and innovation, driven by optimism and a desire to inspire others."
  },
  "status": {
    "code": 200,
    "message": "NPC created successfully."
}
```

## Connecting OpenAI to Unreal Engine

Integrating OpenAI with Unreal Engine allows developers to bring their NPCs to life with realistic and dynamic conversations. Here's a step-by-step guide to achieve this integration:

### Prerequisites

- Unreal Engine 4.26 or later
- OpenAI API Key
- Basic understanding of Unreal Engine and C++

### Steps to Integrate

1. **Install Required Plugins**
   - Download and install the `HTTP` and `JSON` plugins from the Unreal Engine Marketplace.

2. **Setup Your Project**
   - Create a new Unreal Engine project or open an existing one.
   - Enable the `HTTP` and `JSON` plugins in your project settings.

3. **Create an HTTP Request**
   - In your Unreal Engine project, create a new C++ class that will handle the HTTP requests to the OpenAI API.

    ```cpp
    void UMyClass::SendOpenAIRequest(FString Prompt)
    {
        // Create the HTTP request
        TSharedRef<IHttpRequest> Request = FHttpModule::Get().CreateRequest();
        Request->OnProcessRequestComplete().BindUObject(this, &UMyClass::OnResponseReceived);
        Request->SetURL("https://api.openai.com/v1/engines/davinci-codex/completions");
        Request->SetVerb("POST");
        Request->SetHeader("Content-Type", "application/json");
        Request->SetHeader("Authorization", "Bearer YOUR_OPENAI_API_KEY");

        // Create the JSON payload
        FString Payload = FString::Printf(TEXT("{"prompt": "%s", "max_tokens": 150}"), *Prompt);
        Request->SetContentAsString(Payload);

        // Send the request
        Request->ProcessRequest();
    }
    ```

4. **Handle the Response**
   - Implement a method to handle the response from the OpenAI API.

    ```cpp
    void UMyClass::OnResponseReceived(FHttpRequestPtr Request, FHttpResponsePtr Response, bool bWasSuccessful)
    {
        if (bWasSuccessful)
        {
            // Parse the response JSON
            TSharedPtr<FJsonObject> JsonObject;
            TSharedRef<TJsonReader<>> Reader = TJsonReaderFactory<>::Create(Response->GetContentAsString());
            if (FJsonSerializer::Deserialize(Reader, JsonObject))
            {
                // Extract the text response
                FString ResponseText = JsonObject->GetStringField("choices")[0]->AsObject()->GetStringField("text");
                UE_LOG(LogTemp, Log, TEXT("OpenAI Response: %s"), *ResponseText);
            }
        }
    }
    ```

### Sample Integration Diagram

```mermaid
flowchart TD
    A[Start] --> B[Setup Unreal Engine]
    B --> C[Install HTTP Plugin]
    C --> D[Configure OpenAI API]
    D --> E[Create HTTP Request in Unreal]
    E --> F[Set Request Headers]
    F --> G[Send Request to OpenAI]
    G --> H[Receive and Parse Response]
    H --> I[Extract Text Response]
    I --> J[Use Response in NPC Dialogue]
    J --> K[End]


## Conclusion

This repository demonstrates how powerful AI chatbots can be integrated into video games to create engaging and interactive NPCs. By leveraging OpenAI's capabilities and Unreal Engine's robust framework, developers can elevate the gaming experience to new heights.

---

### Contact

For more information, feel free to reach out to us at [contact@example.com](mailto:contact@example.com).

![Footer Image](https://github.com/scottohair/milo-npc-demo/blob/main/footer.png)
