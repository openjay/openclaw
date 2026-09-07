---
name: openai-image-gen
description: Batch-generate images via OpenAI Images API. Random prompt sampler + `index.html` gallery.
homepage: https://platform.openai.com/docs/api-reference/images
metadata:
  {
    "openclaw":
      {
        "emoji": "🎨",
        "requires": { "bins": ["python3"], "env": ["OPENAI_API_KEY"] },
        "primaryEnv": "OPENAI_API_KEY",
        "install":
          [
            {
              "id": "python-brew",
              "kind": "brew",
              "formula": "python",
              "bins": ["python3"],
              "label": "Install Python (brew)",
            },
          ],
      },
  }
---

# OpenAI image generation

Use the bundled scripts/gen.py for the requested image generation, with an
explicit prompt, count and output directory. The source script's default count is
eight and omitted prompts use a random sampler; do not invoke those defaults for
a one-image or specific-content request. Existing OPENAI_API_KEY handling stays
with the configured secret resolver, never printed or placed in prompts.

Inspect script help and the version-specific primary API documentation for the
requested model's supported size/quality/background/output-format/style. Preserve
the user's model choice; source mappings are not proof every historical model is
currently available. Do not silently substitute a model or enlarge a paid batch.

Use a sufficient bounded timeout and reconcile existing outputs after uncertainty
before retrying a possibly billable request. The script records images,
prompts.json and index.html; use its returned actual directory, not a broad glob
that might open a different run. Verify the requested count, mapping and rendered
images where tools permit before delivery. Report any uninspected quality.

Generating files is not publication or permission to upload private reference
assets to another provider. No SDK upgrade, new credentials or changed runtime
settings as an implicit prerequisite. Do not claim success from an empty gallery.
