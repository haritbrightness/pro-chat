# pro-chat

Building an LLM chat screen means handling message history, response rendering, and conversation edits alongside your model API. pro-chat provides React components written in TypeScript for those interface tasks, with a request callback that connects the conversation to your backend.

## Features

- Render Markdown in chat messages without a separate rendering component.
- Display streaming and non-streaming responses from a request callback.
- Let users edit or delete messages and retry requests.
- Cache conversations to maintain chat continuity.
- Control the conversation programmatically through a component ref.

## Installation

Install the package and its UI dependencies in a React application:

```bash
npm install pro-chat antd antd-style react react-dom
```

For a TypeScript application, add the compiler and React type definitions if they are not already installed:

```bash
npm install --save-dev typescript @types/react @types/react-dom
```

**ES modules only.** Use ESM imports in your application.

## Usage

Add this component to an existing React TypeScript application and render it from your app's entry point:

```tsx
import { ProChat } from 'pro-chat';

export default function App() {
  return (
    <div style={{ height: '100vh' }}>
      <ProChat
        request={async (messages) => {
          return new Response(
            `Demo response: received ${messages.length} messages.`,
          );
        }}
      />
    </div>
  );
}
```

This example returns a local response so you can try the interface without a model service. Replace the callback with a request to your backend when you are ready to connect an LLM:

```tsx
<ProChat
  request={async (messages) => {
    const response = await fetch('/api/chat', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ messages }),
    });

    if (!response.ok) {
      throw new Error(`Chat request failed: ${response.status}`);
    }

    return response;
  }}
/>
```

The `/api/chat` endpoint must be implemented by your application. Have it return the assistant's text response, or a compatible text stream, rather than an arbitrary JSON envelope. Keep model-provider credentials on the server.

## License

MIT. pro-chat is based on [ant-design/pro-chat](https://github.com/ant-design/pro-chat), which is also licensed under MIT. Retain the upstream copyright and permission notices in the LICENSE file and in copies or substantial portions of the upstream code.
