# CodeAlpha_Translatior_project


from deep_translator import GoogleTranslator, MyMemoryTranslator

# accept both codes and names
ALIASES = {
    "en": "english", "english": "en",
    "hi": "hindi",   "hindi": "hi",
    "bn": "bengali", "bangla": "bengali", "bengali": "bn",
    "fr": "french",  "french": "fr",
    "de": "german",  "german": "de",
    "es": "spanish", "spanish": "es"
}

def norm_lang(x: str) -> str:
    x = (x or "").strip().lower()
    # return ISO code if user typed name
    if x in ALIASES and len(x) > 2:
        return ALIASES[x]
    # return as-is if already a code we know
    if x in ALIASES:
        return x
    return x  # let library try

def translate(text, src, dest):
    src = "auto" if src in ("", "auto") else norm_lang(src)
    dest = norm_lang(dest)

    try:
        return GoogleTranslator(source=src, target=dest).translate(text)
    except Exception as e:
        # fallback (slower/less accurate but reliable)
        try:
            return MyMemoryTranslator(source=src if src != "auto" else "en",
                                      target=dest).translate(text)
        except Exception as e2:
            raise RuntimeError(f"Both translators failed: {e} | {e2}")

def main():
    print("🌍 Language Translator (type 'exit' to quit)\n")
    while True:
        text = input("Text: ").strip()
        if text.lower() == "exit":
            break
        src = input("Source language (e.g., en/hi/bn or 'auto'): ").strip() or "auto"
        dest = input("Target language (e.g., en/hi/bn): ").strip()

        try:
            out = translate(text, src, dest)
            print("\n--- Result ---")
            print(f"Original ({src}): {text}")
            print(f"Translated ({dest}): {out}\n")
        except Exception as e:
            print(f"⚠️ Error: {e}\n")

if __name__ == "__main__":
    main()
