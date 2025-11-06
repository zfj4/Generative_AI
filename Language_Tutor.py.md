### Language Tutor
This Python code uses the Gemini API to role prompt with system instructions and submit user prompts for second language tutoring. It initially allows for French and Spanish tutoring.
```
# Run this if the import genai statement throws an exception.
#%pip install --upgrade --quiet google-genai

from IPython.display import Markdown, display
from google import genai
from google.genai.types import GenerateContentConfig

API_KEY = "PAst3Y0urrEAlaP1KeyF-r0MGooGLEapisTuD10"
client = genai.Client(api_key=API_KEY)
model_name = "gemini-flash-latest"

# The initial instructions serve as a Role Prompt
def specifyLanguage(language, language_strings):
    system_instruction = f"""You are a {language} tutor for an American high school student.
The student will prompt you with a word or words in either {language} or English, followed by an attempt at a translation.
You will evaluate the translation for both literal and idiomatic accuracy and suggest better and more idiomatic translations when applicable.
You will also offer grammar and vocabulary insights based on the translation.
You will give your feedback in English.
    """
    language_strings.append(system_instruction)
    if language.capitalize() == 'French':
        language_strings.append("Bonjour! Je suis votre tuteur de français. Quels mots aimeriez-vous traduire? (Pour quitter, tapez 'À la prochaine')")
        language_strings.append("Votre traducion:")
        language_strings.append('à la prochaine')
        language_strings.append("Quels autres mots aimeriez-vous traduire?")
    elif language.capitalize() == 'Spanish':
        language_strings.append("¡Hola! Soy su tutor de francés. ¿Qué palabras le gustaría traducir? (Para salir, escriba 'Hasta pronto')")
        language_strings.append("Su traducción:")
        language_strings.append('hasta pronto')
        language_strings.append("¿Qué otras palabras le gustaría traducir?")
    else:
        display(Markdown(f"Sorry, I am not a {language} tutor."))
        language_strings.append(None)
        language_strings.append(None)
        language_strings.append(None)
        language_strings.append(None)
        return False

    return True

language_strings = []
do_while = specifyLanguage(input('I am your language tutor. Which language are you learning? ').capitalize(), language_strings)
system_instruction = language_strings[0]
greeting = language_strings[1]
yourtranslation = language_strings[2]
while do_while:
    display(Markdown(greeting))
    wordsone = input(' ')
    if language_strings[3] in wordsone.lower():
        break
    display(Markdown(wordsone))
    display(Markdown(yourtranslation))
    wordstwo = input(' ')
    response = client.models.generate_content(
        model=model_name,
        contents = wordsone + '\n\r\n\r' + wordstwo,
        config={
            "system_instruction": system_instruction,
            # Other settings (temperature, top_k, etc.) can also be added here.
        }
    )
    greeting = response.text + '\n\r\n\r' + language_strings[4]
```
