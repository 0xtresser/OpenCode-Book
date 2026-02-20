# *Deep Dive into OpenCode* — Table of Contents and Outline

> **Model**: claude-opus-4-6 (anthropic/claude-opus-4-6)
> **Tools**: OpenCode
> **Generated**: 2025-02-17
> **GitHub**: https://github.com/0xtresser/OpenCode-Book

---

## Language

[中文版本](README-ZH.md)

## Book Structure Overview

As of OpenCode version 1.2.5.

This book is divided into **five parts and eighteen chapters**, systematically covering OpenCode's architecture design and implementation principles — from foundational concepts to source code analysis, from core mechanisms to ecosystem extensions, and from theory to practice.

| Part | Chapter Range | Core Topics | Est. Word Count |
|------|--------------|-------------|-----------------|
| Part I: Foundations | Chapters 1–3 | Background, Setup, Overall Architecture | ~15,000 words |
| Part II: Core Architecture | Chapters 4–8 | Session, Tool, Agent, Provider, MCP | ~30,000 words |
| Part III: Key Mechanisms | Chapters 9–12 | Permissions, Snapshot, Event Bus, TUI | ~20,000 words |
| Part IV: Ecosystem | Chapters 13–16 | Plugin System, oh-my-opencode, IDE Extensions | ~20,000 words |
| Part V: Practice | Chapters 17–18 | Hands-on Labs, Design Philosophy & Future Outlook | ~15,000 words |

---

## Part I: Foundations

### Chapter 1: The Era of AI Programming Assistants

> Guiding readers to understand the technical background and industry context behind OpenCode's creation.

- **[1.1 From Auto-Completion to Agentic Coding](EN/Chapter_01_AI_Programming_Assistant_Background/1.1_From_Auto-Completion_to_Agentic_Coding.md)**
- **[1.2 AI Programming Assistant Product Landscape](EN/Chapter_01_AI_Programming_Assistant_Background/1.2_AI_Programming_Assistant_Product_Landscape.md)**
- **[1.3 OpenCode's Positioning and Design Philosophy](EN/Chapter_01_AI_Programming_Assistant_Background/1.3_OpenCode_Positioning_and_Design_Philosophy.md)**

---

### Chapter 2: Project Structure and Development Environment

> Setting up the development environment and building a macro-level understanding of the project structure.

- **[2.1 Obtaining Source Code and Environment Setup](EN/Chapter_02_Project_Structure_and_Development_Environment/2.1_Obtaining_Source_Code_and_Environment_Setup.md)**
- **[2.2 Monorepo Structure Analysis](EN/Chapter_02_Project_Structure_and_Development_Environment/2.2_Monorepo_Structure_Analysis.md)**
- **[2.3 Core Package `packages/opencode/src/` Module Map](EN/Chapter_02_Project_Structure_and_Development_Environment/2.3_Core_Package_Module_Map.md)**

---

### Chapter 3: Overall Architecture Design

> Understanding OpenCode's architectural layers and data flow from a global perspective.

- **[3.1 Client-Server Architecture](EN/Chapter_03_Overall_Architecture_Design/3.1_Client-Server_Architecture.md)**
- **[3.2 Core Data Flow: From User Input to AI Response](EN/Chapter_03_Overall_Architecture_Design/3.2_Core_Data_Flow.md)**
- **[3.3 Multi-Project and Multi-Worktree Support](EN/Chapter_03_Overall_Architecture_Design/3.3_Multi-Project_and_Multi-Worktree_Support.md)**
- **[3.4 Layered Configuration System Design](EN/Chapter_03_Overall_Architecture_Design/3.4_Layered_Configuration_System_Design.md)**

---

## Part II: Core Architecture

### Chapter 4: Session System

> The session is OpenCode's core abstraction — understanding it means understanding the entire conversation management mechanism.

- **[4.1 Session Data Model](EN/Chapter_04_Session_System/4.1_Session_Data_Model.md)**
- **[4.2 Message Model](EN/Chapter_04_Session_System/4.2_Message_Model.md)**
- **[4.3 SessionPrompt: The Session Entry Point](EN/Chapter_04_Session_System/4.3_SessionPrompt_Entry_Point.md)**
- **[4.4 SessionProcessor: The Core of the Agentic Loop](EN/Chapter_04_Session_System/4.4_SessionProcessor_Core_Loop.md)**
- **[4.5 Compaction: Context Window Management](EN/Chapter_04_Session_System/4.5_Compaction_Context_Window_Management.md)**
- **[4.6 Session State Management](EN/Chapter_04_Session_System/4.6_Session_State_Management.md)**

---

### Chapter 5: Tool System

> Tools are the only way Agents interact with the external world.

- **[5.1 Tool Abstraction Layer Design](EN/Chapter_05_Tool_System/5.1_Tool_Abstraction_Layer_Design.md)**
- **[5.2 ToolRegistry: Registration and Discovery](EN/Chapter_05_Tool_System/5.2_ToolRegistry_Registration_and_Discovery.md)**
- **[5.3 Built-in Tools Source Code Analysis](EN/Chapter_05_Tool_System/5.3_Built-in_Tools_Source_Code_Analysis.md)**
- **[5.4 Permission Checks in Tool Invocations](EN/Chapter_05_Tool_System/5.4_Permission_Checks_in_Tool_Invocations.md)**

---

### Chapter 6: Agent System

> An Agent is the abstraction that gives an LLM its "personality" and "capability boundaries."

- **[6.1 Agent Data Model](EN/Chapter_06_Agent_System/6.1_Agent_Data_Model.md)**
- **[6.2 Built-in Agent: build](EN/Chapter_06_Agent_System/6.2_Built-in_Agent_build.md)**
- **[6.3 Agent Auxiliary Functions](EN/Chapter_06_Agent_System/6.3_Agent_Auxiliary_Functions.md)**
- **[6.4 System Prompt Architecture](EN/Chapter_06_Agent_System/6.4_System_Prompt_Architecture.md)**
- **[6.5 Custom Agent Configuration](EN/Chapter_06_Agent_System/6.5_Custom_Agent_Configuration.md)**

---

### Chapter 7: Provider (Multi-Model Adaptation Layer)

> Understanding how OpenCode uniformly interfaces with 20+ LLM Providers.

- **[7.1 Vercel AI SDK Integration](EN/Chapter_07_Provider_Multi_Model_Adaptation_Layer/7.1_Vercel_AI_SDK_Integration.md)**
- **[7.2 Provider Registration and Resolution](EN/Chapter_07_Provider_Multi_Model_Adaptation_Layer/7.2_Provider_Registration_and_Resolution.md)**
- **[7.3 Model Metadata System](EN/Chapter_07_Provider_Multi_Model_Adaptation_Layer/7.3_Model_Metadata_System.md)**
- **[7.4 ProviderTransform: Differentiated Model Adaptation](EN/Chapter_07_Provider_Multi_Model_Adaptation_Layer/7.4_ProviderTransform_Differentiated_Adaptation.md)**
- **[7.5 LLM Module: Streaming Call Implementation](EN/Chapter_07_Provider_Multi_Model_Adaptation_Layer/7.5_LLM_Module_Streaming_Call_Implementation.md)**

---

### Chapter 8: MCP (Model Context Protocol)

> MCP is the core protocol for OpenCode to connect with external tool ecosystems.

- **[8.1 MCP Protocol Introduction](EN/Chapter_08_MCP/8.1_MCP_Protocol_Introduction.md)**
- **[8.2 MCP Client Implementation](EN/Chapter_08_MCP/8.2_MCP_Client_Implementation.md)**
- **[8.3 MCP OAuth Authentication](EN/Chapter_08_MCP/8.3_MCP_OAuth_Authentication.md)**
- **[8.4 MCP Configuration](EN/Chapter_08_MCP/8.4_MCP_Configuration.md)**

---

## Part III: Key Mechanisms

### Chapter 9: Permission Control System

> Security is the lifeline of AI Agents — deeply understanding how OpenCode controls Agent behavior boundaries.

- **[9.1 Permission Model Design](EN/Chapter_09_Permission_Control_System/9.1_Permission_Model_Design.md)**
- **[9.2 Permission Evaluation Engine](EN/Chapter_09_Permission_Control_System/9.2_Permission_Evaluation_Engine.md)**
- **[9.3 Permission Configuration Hierarchy](EN/Chapter_09_Permission_Control_System/9.3_Permission_Configuration_Hierarchy.md)**
- **[9.4 Permission Request Interaction Flow](EN/Chapter_09_Permission_Control_System/9.4_Permission_Request_Interaction_Flow.md)**
- **[9.5 Permission Strategies for Each Tool](EN/Chapter_09_Permission_Control_System/9.5_Permission_Strategies_for_Each_Tool.md)**

---

### Chapter 10: Snapshot and File System

> Understanding how OpenCode safely manages file changes and rollbacks.

- **[10.1 Git Snapshot System](EN/Chapter_10_Snapshot_and_File_System/10.1_Git_Snapshot_System.md)**
- **[10.2 File System Tools](EN/Chapter_10_Snapshot_and_File_System/10.2_File_System_Tools.md)**
- **[10.3 Patch System](EN/Chapter_10_Snapshot_and_File_System/10.3_Patch_System.md)**
- **[10.4 Worktree Management](EN/Chapter_10_Snapshot_and_File_System/10.4_Worktree_Management.md)**

---

### Chapter 11: Event Bus and Scheduler

> Understanding the core mechanism for decoupled inter-module communication.

- **[11.1 Event Bus](EN/Chapter_11_Event_Bus_and_Scheduler/11.1_Event_Bus.md)**
- **[11.2 Key Event List](EN/Chapter_11_Event_Bus_and_Scheduler/11.2_Key_Event_List.md)**
- **[11.3 Scheduler: Timed Task Scheduling](EN/Chapter_11_Event_Bus_and_Scheduler/11.3_Scheduler_Timed_Task_Scheduling.md)**

---

### Chapter 12: Command Line Interface and TUI

> Understanding OpenCode's frontend implementation from a user interaction perspective.

- **[12.1 CLI Startup Flow](EN/Chapter_12_CLI_and_TUI/12.1_CLI_Startup_Flow.md)**
- **[12.2 TUI Implementation](EN/Chapter_12_CLI_and_TUI/12.2_TUI_Implementation.md)**
- **[12.3 Web UI](EN/Chapter_12_CLI_and_TUI/12.3_Web_UI.md)**
- **[12.4 Non-Interactive Mode](EN/Chapter_12_CLI_and_TUI/12.4_Non-Interactive_Mode.md)**

---

## Part IV: Ecosystem

### Chapter 13: Plugin System

> Understanding the core of OpenCode's extensibility — the Plugin architecture.

- **[13.1 Plugin Interface Definition](EN/Chapter_13_Plugin_System/13.1_Plugin_Interface_Definition.md)**
- **[13.2 Plugin Lifecycle Hooks](EN/Chapter_13_Plugin_System/13.2_Plugin_Lifecycle_Hooks.md)**
- **[13.3 Plugin Loading Mechanism](EN/Chapter_13_Plugin_System/13.3_Plugin_Loading_Mechanism.md)**
- **[13.4 OpenCode SDK](EN/Chapter_13_Plugin_System/13.4_OpenCode_SDK.md)**

---

### Chapter 14: Skill System

> Skills are modular extension units for Agent capabilities.

- **[14.1 Skill Definition and Structure](EN/Chapter_14_Skill_System/14.1_Skill_Definition_and_Structure.md)**
- **[14.2 Skill Loading Flow](EN/Chapter_14_Skill_System/14.2_Skill_Loading_Flow.md)**
- **[14.3 Command System](EN/Chapter_14_Skill_System/14.3_Command_System.md)**

---

### Chapter 15: oh-my-opencode Deep Dive

> As the most complex third-party Plugin in the OpenCode ecosystem, oh-my-opencode demonstrates the full power of the Plugin system.

- **[15.1 Project Overview and Architecture](EN/Chapter_15_oh-my-opencode_Deep_Dive/15.1_Project_Overview_and_Architecture.md)**
- **[15.2 Plugin Entry Point and Initialization](EN/Chapter_15_oh-my-opencode_Deep_Dive/15.2_Plugin_Entry_Point_and_Initialization.md)**
- **[15.3 Multi-Agent System](EN/Chapter_15_oh-my-opencode_Deep_Dive/15.3_Multi-Agent_System.md)**
- **[15.4 Custom Tool System](EN/Chapter_15_oh-my-opencode_Deep_Dive/15.4_Custom_Tool_System.md)**
- **[15.5 Hook System (53 Hooks Explained)](EN/Chapter_15_oh-my-opencode_Deep_Dive/15.5_Hook_System.md)**
- **[15.6 Feature Modules](EN/Chapter_15_oh-my-opencode_Deep_Dive/15.6_Feature_Modules.md)**
- **[15.7 Plugin Interface Implementation](EN/Chapter_15_oh-my-opencode_Deep_Dive/15.7_Plugin_Interface_Implementation.md)**
- **[15.8 Embedded MCP Services](EN/Chapter_15_oh-my-opencode_Deep_Dive/15.8_Embedded_MCP_Services.md)**

---

### Chapter 16: IDE Extensions and ACP

> Understanding how OpenCode integrates with various IDEs.

- **[16.1 VSCode Extension](EN/Chapter_16_IDE_Extensions_and_ACP/16.1_VSCode_Extension_Implementation.md)**
- **[16.2 Zed Extension](EN/Chapter_16_IDE_Extensions_and_ACP/16.2_Zed_Extension_Implementation.md)**
- **[16.3 ACP (Agent Client Protocol)](EN/Chapter_16_IDE_Extensions_and_ACP/16.3_ACP_Protocol_Deep_Analysis.md)**

---

## Part V: Practice

### Chapter 17: Hands-on Labs

> Through five progressive experiments, build key components of an AI programming assistant from scratch.

- **[17.1 Lab 1: Implementing a Simple LLM CLI Chat Tool](EN/Chapter_17_Hands-on_Labs/17.1_Lab1_LLM_CLI_Chat_Tool.md)**
- **[17.2 Lab 2: Adding Tool Use Capability to the CLI Tool](EN/Chapter_17_Hands-on_Labs/17.2_Lab2_Tool_Use_Capability.md)**
- **[17.3 Lab 3: Building an MCP Server](EN/Chapter_17_Hands-on_Labs/17.3_Lab3_Building_MCP_Server.md)**
- **[17.4 Lab 4: Writing an OpenCode Plugin](EN/Chapter_17_Hands-on_Labs/17.4_Lab4_Writing_OpenCode_Plugin.md)**
- **[17.5 Lab 5: Building a Simplified Multi-Agent Orchestration System](EN/Chapter_17_Hands-on_Labs/17.5_Lab5_Simplified_Multi-Agent_Orchestration_System.md)**

---

### Chapter 18: Design Philosophy, Best Practices, and Future Outlook

> Summarizing the entire book, distilling design wisdom, and looking ahead to future directions.

- **[18.1 Review of OpenCode's Key Design Decisions](EN/Chapter_18_Design_Philosophy_and_Best_Practices/18.1_Review_of_OpenCode_Key_Design_Decisions.md)**
- **[18.2 Design Insights from oh-my-opencode](EN/Chapter_18_Design_Philosophy_and_Best_Practices/18.2_Design_Insights_from_oh-my-opencode.md)**
- **[18.3 Best Practices for Building AI Programming Assistants](EN/Chapter_18_Design_Philosophy_and_Best_Practices/18.3_Best_Practices_for_Building_AI_Programming_Assistants.md)**
- **[18.4 Future Outlook](EN/Chapter_18_Design_Philosophy_and_Best_Practices/18.4_Future_Outlook.md)**

---

## Appendices

### Appendix A: OpenCode Configuration Reference Manual
- Complete list and description of `opencode.json` configuration options

### Appendix B: oh-my-opencode Configuration Reference Manual
- Complete list and description of `oh-my-opencode.json` configuration options

### Appendix C: Built-in Tool API Reference
- Parameters, return values, and permission requirements for each built-in tool

### Appendix D: MCP Protocol Quick Reference
- Core concepts, transport methods, message formats

### Appendix E: Glossary
- Definitions of terms: LLM, Token, Context Window, Function Calling, Agent, MCP, ACP, LSP, AST, SSE, Monorepo, TUI, etc.

### Appendix F: Recommended Reading and Resources
- Official documentation links
- Related papers and articles
- Recommended open-source projects

---

## Source Code File Index

> Quick reference table for key source code files covered in this book

| Chapter | Key Files |
|---------|-----------|
| Chapter 4: Session | `session/index.ts`, `session/processor.ts`, `session/llm.ts`, `session/compaction.ts`, `session/system.ts`, `session/instruction.ts`, `session/prompt.ts`, `session/message-v2.ts`, `session/retry.ts`, `session/revert.ts`, `session/status.ts`, `session/summary.ts`, `session/todo.ts` |
| Chapter 5: Tool | `tool/tool.ts`, `tool/registry.ts`, `tool/truncation.ts`, `tool/bash.ts`, `tool/edit.ts`, `tool/read.ts`, `tool/write.ts`, `tool/glob.ts`, `tool/grep.ts`, `tool/task.ts`, `tool/lsp.ts`, `tool/webfetch.ts`, `tool/websearch.ts`, `tool/todo.ts`, `tool/skill.ts`, `tool/plan.ts`, `tool/batch.ts`, `tool/question.ts`, `tool/apply_patch.ts`, `tool/multiedit.ts`, `tool/codesearch.ts` |
| Chapter 6: Agent | `agent/agent.ts`, `agent/prompt/*.txt`, `session/prompt/*.txt` |
| Chapter 7: Provider | `provider/provider.ts`, `provider/transform.ts`, `provider/models.ts`, `provider/auth.ts`, `provider/error.ts`, `provider/sdk/copilot/` |
| Chapter 8: MCP | `mcp/index.ts`, `mcp/auth.ts`, `mcp/oauth-provider.ts`, `mcp/oauth-callback.ts` |
| Chapter 9: Permission | `permission/next.ts`, `permission/index.ts`, `permission/arity.ts` |
| Chapter 10: Snapshot | `snapshot/index.ts`, `file/index.ts`, `file/ignore.ts`, `file/ripgrep.ts`, `file/watcher.ts`, `patch/index.ts`, `worktree/index.ts` |
| Chapter 11: Bus | `bus/index.ts`, `bus/bus-event.ts`, `bus/global.ts`, `scheduler/index.ts` |
| Chapter 12: CLI/TUI | `cli/bootstrap.ts`, `cli/cmd/*.ts`, `cli/cmd/tui/app.tsx`, `cli/cmd/tui/component/`, `cli/cmd/tui/routes/` |
| Chapter 13: Plugin | `plugin/index.ts`, `plugin/codex.ts`, `plugin/copilot.ts` |
| Chapter 14: Skill | `skill/index.ts`, `skill/skill.ts`, `skill/discovery.ts`, `command/index.ts` |
| Chapter 15: oh-my-opencode | `oh-my-opencode/src/index.ts`, `agents/*.ts`, `tools/*`, `hooks/*`, `features/*`, `plugin/*`, `mcp/*`, `config/*` |
| Chapter 16: IDE/ACP | `acp/agent.ts`, `acp/session.ts`, `acp/types.ts`, `sdks/vscode/`, `extensions/zed/` |
