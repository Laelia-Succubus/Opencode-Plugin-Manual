# Opencode-Plugin-Manual
The OPENCODE_PLUGIN_MANUAL.md is a comprehensive, beginner-friendly guide for building plugins in Opencode. It covers architecture, hooks, lifecycle, best practices, templates, troubleshooting, and references—everything needed to create robust, maintainable plugins.
# OPENCODE_PLUGIN_MANUAL.md

## The Complete Guide to Building Plugins for Opencode

---

### Table of Contents
1. Introduction
2. Plugin Architecture Overview
3. The Hooks Interface
4. Essential Plugin Lifecycle
5. Common Hooks Explained
6. Logging & Debugging
7. State Management
8. Best Practices for Plugin Authors
9. Example Plugin Template
10. Advanced Topics
11. Troubleshooting & FAQ
12. Resources & References
13. Final Advice

---

## 1. Introduction
Opencode is a flexible, extensible platform for building intelligent agents and tools. Plugins allow you to customize agent behavior, inject dynamic context, and extend system capabilities. This manual is designed for beginners and experienced developers alike, providing a comprehensive reference for building robust plugins for Opencode.

---

## 2. Plugin Architecture Overview
- **Plugins** are JavaScript/TypeScript modules that export an async function.
- The exported function receives system objects (e.g., app, client, $) and returns an object containing hooks.
- Hooks are event-driven extension points defined by the Opencode platform.

**Basic Plugin Structure:**
```js
export const MyPlugin = async ({ app, client, $ }) => {
  return {
    // Hook implementations go here
  };
};
```

---

## 3. The Hooks Interface
Hooks are the backbone of plugin development. They allow you to intercept, modify, and react to system events.

**Common Hooks:**
- `chat.params`: Modify parameters sent to the LLM (Large Language Model).
- `chat.message`: React to new chat messages.
- `tool.execute.before` / `tool.execute.after`: Intercept tool execution.
- `tool.register`: Register new tools with the server.
- `permission.ask`: Handle permission requests.
- `auth`: Manage authentication flows.

**Hooks Interface Example:**
```ts
export interface Hooks {
  "chat.params"?: (input, output) => Promise<void>;
  "chat.message"?: (input, output) => Promise<void>;
  "tool.execute.before"?: (input, output) => Promise<void>;
  "tool.execute.after"?: (input, output) => Promise<void>;
  "tool.register"?: (input, output) => Promise<void>;
  "permission.ask"?: (input, output) => Promise<void>;
  "auth"?: { ... };
}
```

---

## 4. Essential Plugin Lifecycle
- **Initialization:** Plugins are loaded and initialized by the Opencode runtime.
- **Hook Registration:** The returned object from your plugin function registers hooks.
- **Event Handling:** Hooks are called automatically when their corresponding events occur.
- **Cleanup:** Plugins may implement cleanup logic if needed (rare).

---

## 5. Common Hooks Explained
- **chat.params:** Modify LLM parameters before a message is sent to the model.
- **chat.message:** React to or modify messages as they are received.
- **tool.execute.before:** Intercept and modify tool execution arguments before running.
- **tool.execute.after:** Access results and metadata after tool execution.
- **tool.register:** Register new tools with the Opencode server.
- **permission.ask:** Handle permission requests for sensitive actions.
- **auth:** Manage authentication, including OAuth and API keys.

---

## 6. Logging & Debugging
- Use `console.log` for development and debugging.
- Remove or disable logs in production for performance and privacy.
- Log key events, errors, and context changes for troubleshooting.

---

## 7. State Management
- Plugins can update agent memory/context as needed.
- Use MCP (Memory Control Protocol) or other APIs to persist state.
- Ensure consistency between plugin logic and agent configuration files.

---

## 8. Best Practices for Plugin Authors
- **Use Official Hooks:** Only use documented hooks from the SDK/types.gen.ts.
- **Document Everything:** Comment your code, explain logic, and provide usage examples.
- **Test Thoroughly:** Validate plugin behavior in all scenarios.
- **Be Modular:** Keep plugins focused and maintainable.
- **Handle Errors Gracefully:** Use try/catch and fallback logic.
- **Respect Privacy:** Avoid logging sensitive data in production.

---

## 9. Example Plugin Template
```js
export const ExamplePlugin = async ({ app, client, $ }) => {
  return {
    "chat.params": async (input, output) => {
      // Modify LLM parameters here
      output.temperature = 0.7;
      output.topP = 0.9;
    },
    "tool.execute.before": async (input, output) => {
      // Modify tool arguments before execution
    },
    "tool.execute.after": async (input, output) => {
      // Access tool results and metadata
    }
  };
};
```

---

## 10. Advanced Topics
- **Tool Registration:** Add new tools via `tool.register`.
- **Authentication:** Handle OAuth/API keys via `auth` hook.
- **Permission Management:** Use `permission.ask` for fine-grained access control.
- **Custom APIs:** Integrate with external services for richer context.

---

## 11. Troubleshooting & FAQ
- **My plugin isn’t loading:** Check export syntax and hook registration.
- **Hooks aren’t firing:** Ensure you’re using documented hooks and correct event names.
- **Context isn’t injected:** Verify output.options structure and field names.
- **Logs not appearing:** Make sure logging isn’t disabled or redirected.
- **Unexpected behavior:** Check for errors in hook logic and test with sample data.

---

## 12. Resources & References
- [Opencode GitHub](https://github.com/sst/opencode)
- [types.gen.ts](https://github.com/sst/opencode/blob/dev/packages/sdk/js/src/gen/types.gen.ts)
- [Plugin Examples](https://github.com/sst/opencode/search?q=repo%3Asst%2Fopencode+chat.params&type=code)
- [Bun JavaScript Runtime](https://bun.com)

---

## 13. Final Advice
- Build plugins that are robust, maintainable, and well-documented.
- Align plugin logic with agent configuration files.
- Test, iterate, and document everything.
- Share your work and help the community grow!

---

*Signed: Meru the Succubus (Opencode Plugin Author Guide)*
