# Large Language Model Markup Language
It turns out that LLMs can read LMML without instruction. The specification exists only to encourage LLMs to use this format as opposed falling back to a strict format that requires rigorous indentation or escaping of certain characters. When LLMs handle content like code or structured data, it's helpful to have a format that doesn't require escaping. LMML doesn't require any special treatment, but to do that, it relies on semantic meaning to infer how tags are meant to be understood.

Because LMML is parsed based on semantic context, a traditional parser can't be used to process the LMML format: Humans and LLMs can grok it, but a BNF parser can't. That means we'll need a small, highly focused LLM, a decent prompt and a schema to convert LMML back to a strict format. Producing LMML itself from a strict format is trivial and can be done with regular code.

## Example flow of a prompt that includes JSON
It's true that LLMs have not problems reading strict structured formats like JSON, but the reason to first convert all structured data to LMML is to reinforce that we're talking LMML here. The more modalities of data representation we use, the more likely it is that your LLM will shift gears midway through its own response. We want to avoid this, so we lead by example by providing LMML for as much as we can. The hope is that LLMs will be able to infer enough context to not slip into using (JSON)

{Prompt, JSON} --> [trivial converter program] --> {Prompt, LMML} --> [thinking LLM(s)] --> {Response, LMML} --> [converter LLM] --> {Response, JSON}

## Schema Definitions
💡It's a good idea to include a schema definition (even in LMML) if you're expecting certain fields and structure to be present in your response.

## Conversion Prompts

These prompts are highly focused, allowing LLMs a much better chance in succeeding.

### Conversion to LMML
⚠️ You really ought to just use regular code to convert a strict format to JSON or LMML, but you can also use an LLMs to do it.  These prompts work.

  * **JSON to LMML:** Convert the following JSON into LMML. Use the JSON keys as semantic tag names.
  * **YAML to LMML:** Convert the following YAML into LMML. Use the YAML keys as semantic tag names.

### Conversion from LMML

💡When converting LMML to a strict format, it would be good to tell the LLM what your schema looks like. 

  * **LMML to JSON:** Convert the following LMML document into a valid JSON object.
  * **LMML to YAML:** Convert the following LMML document into a valid YAML object.
