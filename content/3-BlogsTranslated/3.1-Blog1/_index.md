---
title: "Blog 1"
date: 2024-01-01T00:00:00+07:00
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Exploring the latest features of the Amazon Q Developer CLI

**by Brian Beach on 20 MAY 2025 in Amazon Q Developer, Announcements**

It's been a few weeks since my last post about the Amazon Q Developer Command Line Interface (CLI), and I'm excited to share all the great new features and improvements the team has been working on. The CLI has been evolving rapidly with a focus on enhancing user experience, improving context management, and adding powerful new capabilities. In this post, I'll walk you through the most significant changes that make the Amazon Q Developer CLI even more powerful and user-friendly.

## Conversation Persistence

One of the most requested features has been the ability to persist conversations, and I'm thrilled to share that this is now available. With the new `q chat --resume` command, your conversations are now automatically saved by a working directory. This means you can pick up right where you left off when you return to a project, without having to rebuild context or repeat information.

Q Developer has also added two new commands to give you more control over your conversation state:

- `/save` allows you to explicitly save the current conversation state
- `/load` lets you restore a previously saved conversation

These commands make it easier to manage multiple conversation threads related to different aspects of your project. You can save a conversation about one feature, switch to working on something else, and then load the previous conversation when you're ready to continue.

## MCP and Tool Use Enhancements

The Model Context Protocol (MCP) is a key part of the Amazon Q Developer CLI, allowing for extensibility through additional tools and servers. Q Developer has made several improvements to how MCP servers are loaded and managed:

First, Q Developer has implemented background MCP server loading, which significantly improves startup time for `q chat`. Instead of waiting for all MCP servers to initialize before you can start interacting with Q Developer, the CLI now loads servers in the background while you begin your conversation. This means you can start working immediately, with tools becoming available as their servers finish loading.

The team has also added a new subcommand, `q mcp`, which provides a dedicated interface for updating and managing your MCP server configuration. This makes it easier to add, remove, or modify the MCP servers that extend your CLI's capabilities.

For more granular control over which tools can be used, Q Developer has added the `/tools` command in `q chat`. This allows you to manage permissions for individual tools, giving you more control over what Q Developer can do in your environment. You can also reset permissions for a specific tool if you change your mind.

## Improved Context Control

Context is crucial for getting the most out of Q Developer, and the team has made several improvements to how you can manage and view context:

The file selection in `q chat`'s fuzzy finder is now git-aware, making it easier to include relevant files from your repository. This is particularly useful when working with large codebases, as it helps you focus on the files that matter for your current task.

Q Developer has added fuzzy search for slash commands with `Ctrl + s`, allowing you to quickly find and execute commands without remembering their exact syntax. This makes the CLI more accessible, especially for new users or those who don't use certain commands frequently.

The `/context show --expand` command has been improved to provide more detailed information about the current context, helping you understand what Q Developer knows about your environment. The team has also enhanced the context file display in `q chat` to make it more informative and easier to read.

One of the most exciting additions is the new capability for dynamically adding context to messages with context hooks. This allows the CLI to automatically include relevant context based on your conversation, improving the quality of responses without requiring manual context management.

## Context Window Awareness and Optimization

As conversations grow longer, managing the context window becomes increasingly important. Q Developer has added two new commands to help with this:

- `/usage` displays an estimate of the context window usage, helping you understand how much of the available context space you're using
- `/compact` summarizes the conversation history, allowing you to reduce the size of the context while preserving the important information

These tools help you make the most of the available context window, ensuring that Q Developer has access to the most relevant information without running into token limits.

## Image Support

I'm particularly excited to announce that `q chat` now supports images! This opens up a whole new dimension of interaction, allowing you to share screenshots, diagrams, or other visual information with Q Developer. This can be incredibly useful for debugging UI issues, discussing design concepts, or explaining complex ideas that are difficult to convey through text alone.

## Editor for Long Prompts

For complex queries or detailed instructions, you may want multiple paragraphs. Q Developer supports `Ctrl + j`, allowing you to add a newline character to the prompt. In addition, the team has added the `/editor` command, which opens your configured text editor for composing prompts. This makes it much easier to craft detailed, multi-paragraph prompts or to edit and refine your questions before sending them to Q Developer.

## Expanded Region Support

I'm happy to announce that Q Developer has expanded its regional availability. Professional tier users can now access Q Developer in the Frankfurt region (eu-central-1). This expansion is part of Q Developer's ongoing effort to provide lower latency and better service to customers across the globe. By adding support for the Frankfurt region, Amazon Q Developer is more accessible to European customers, allowing them to benefit from reduced latency and improved performance.

## Ability to Manage Issues in CLI

Amazon Q Developer has made it easier to report issues directly from the CLI with two new features:

- The `/issue` command in `q chat` allows you to create new GitHub issues
- The `report_issue` tool provides a programmatic way for Q Developer to help you create detailed issue reports

These features streamline the feedback process, making it easier for you to report bugs or request features, and for the team to improve the CLI based on your input.

## Keeping Up with Future Changes

To help you stay informed about new features and improvements, Q Developer has added a `--changelog` flag to the `q version` command. This displays the change log directly from the CLI, making it easy to see what's new without having to visit the GitHub repository or read blog posts like this one.

## Conclusion

The Amazon Q Developer CLI continues to evolve rapidly, with new features and improvements that make it an even more powerful tool for developers. From conversation persistence to image support, these updates reflect Q Developer's commitment to building a CLI that helps you be more productive and effective in your daily work. I encourage you to try out these new features by installing the Amazon Q Developer CLI. Thank you for your continued support and feedback, which helps make Amazon Q Developer better every day.
