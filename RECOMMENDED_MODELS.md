# Recommended Models for the WinWin.travel MCP Server

## Recommended Minimum Model Capability

To achieve reliable results when using the WinWin.travel MCP Server, we recommend using language models with an **Intelligence Index (II) at least equivalent to Claude Sonnet 4.6**.

Models below this capability level may still work for simple requests, but they demonstrate a significantly higher rate of reasoning errors when interacting with MCP tools and processing complex travel data.

> **Minimum recommended Intelligence Index:** Claude Sonnet 4.6 (or equivalent)

## Known Issues with Lower Intelligence Models

Models with an Intelligence Index below the recommended threshold may produce inaccurate or inconsistent results. The most common issues include:

### Incorrect interpretation of structured data

Lower-capability models may incorrectly understand the relationships between entities returned by MCP tools. For example, they may:

- confuse rooms that belong to different hotels;
- associate room features or amenities with the wrong room;
- mix information from multiple offers into a single response.

### Hallucinated information

Less capable models are more likely to generate information that is not present in the MCP response, including:

- room characteristics that were never returned;
- amenities or services that do not exist;
- incorrect pricing explanations or offer details.

### Ignoring tool instructions

Some models partially or completely ignore the instructions provided in tool descriptions. This may result in:

- using fields incorrectly;
- skipping required processing steps;
- producing answers that do not follow the intended workflow.

### Reduced consistency

Lower Intelligence Index models are generally less reliable when handling:

- large tool responses;
- multi-step reasoning;
- conversations involving multiple MCP tool calls.

This can lead to inconsistent answers even when the underlying MCP data is correct.

## Recommendations

For the best experience when using the WinWin.travel MCP Server:

- Use models with an Intelligence Index **equal to or higher than Claude Sonnet 4.6**.
- Higher-capability models provide significantly more reliable reasoning over complex travel data and better adherence to MCP tool instructions.
- Avoid using models below the recommended threshold for production scenarios where response accuracy is important.
- If lower-capability models must be used, additional validation of generated responses is strongly recommended.
