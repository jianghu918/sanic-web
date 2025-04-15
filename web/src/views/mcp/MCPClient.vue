<template>
    <LayoutCenterPanel>
        <n-layout class="main-layout" style="height: 100vh">
            <div class="mcp-client">
                <h1>MCP Client</h1>
                <p>Connection Status: {{ connectionStatus }}</p>

                <!-- 输入框和按钮 -->
                <div>
                    <input
                        v-model="message"
                        style="width: 280px; height: 25px"
                        placeholder="Enter message to send"
                    />
                    <button
                        @click="sendMessage"
                        style="height: 25px; margin-left: 2px; width: 80px"
                    >
                        Send
                    </button>
                </div>

                <!-- 消息日志 -->
                <div class="message-log">
                    <h3>Message Log:</h3>
                    <ul>
                        {{
                            messages
                        }}
                    </ul>
                </div>
            </div>
        </n-layout>
    </LayoutCenterPanel>
</template>

<script lang="tsx" setup>
import { Client } from '@modelcontextprotocol/sdk/client/index.js'
import { SSEClientTransport } from '@modelcontextprotocol/sdk/client/sse.js' // 假设存在 SseClientTransport

const message = ref('')
const messages = ref('')
const connectionStatus = ref('Disconnected')
//https://github.com/modelcontextprotocol/typescript-sdk
// StdioClientTransport
const transport = new SSEClientTransport(new URL('http://localhost:8000/sse'))

const client = new Client(
    {
        name: 'mcp-client',
        version: '1.0.0'
    },
    {
        capabilities: {
            prompts: {},
            resources: {},
            tools: {}
        }
    }
)

onMounted(async () => {
    await client.connect(transport)
    connectionStatus.value = 'Connected'
    const tools = await client.listTools()
    console.log(tools)

    // const prompts = await client.listPrompts()
    // console.log(prompts)
})

const sendMessage = async () => {
    const result = await client.callTool({
        name: 'get_weather',
        arguments: {
            location: message.value
        }
    })
    messages.value = JSON.stringify(result.content)
    console.log(result.content)
}

// List prompts
// const prompts = await client.listPrompts()

// // Get a prompt
// const prompt = await client.getPrompt('example-prompt', {
//     arg1: 'value'
// })

// List resources
// const resources = await client.listResources()

// // Read a resource
// const resource = await client.readResource('file:///example.txt')
</script>

<style scoped>
.mcp-client {
    font-family: Arial, sans-serif;
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 8px;
    margin-top: 200px;
}

.message-log {
    margin-top: 20px;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    background: #f9f9f9;
    margin: 5px 0;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
}
.main-layout,
.sub-layout {
    height: 100%;
    border-radius: 10px;
    margin-bottom: 10px;
    margin-right: 2px;
    background-color: #ffffff;
}
</style>
