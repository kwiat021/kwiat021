import random
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time

def generate_codes(num_codes):
    codes = []
    for _ in range(num_codes):
        code = ''
        for i in range(16):
            if i > 0 and i % 4 == 0:
                code += '-'
            code += str(random.randint(0, 9))
        codes.append(code)
    return codes

# Funkcja do sprawdzania poprawności kodu PSC na stronie PSC
def check_psc(code):
    # Tutaj można dodać prawdziwe sprawdzanie kodów PSC na stronie
    # W tym przykładzie zwraca True dla każdego kodu
    return True

# Generowanie kodów PSC
num_codes = int(input("Podaj liczbę kodów do wygenerowania: "))
generated_codes = generate_codes(num_codes)

# Ścieżka do pliku wykonywalnego przeglądarki (np. chromedriver dla Chrome)
driver_path = 'ścieżka/do/chromedriver'

# Inicjalizacja przeglądarki
driver = webdriver.Chrome(executable_path=driver_path)

# Otwórz stronę PSC
driver.get('https://psc.pl')

# Poczekaj, aż strona się załaduje
time.sleep(3)

# Załóżmy, że pole, w które należy wpisać kod PSC, ma atrybut 'id' o wartości 'psc_code_input'
# Poniżej znajduje się kod symulujący wprowadzenie kodu PSC na stronie
for code in generated_codes:
    psc_input = driver.find_element_by_id('psc_code_input')
    psc_input.clear()
    psc_input.send_keys(code)
    psc_input.send_keys(Keys.RETURN)

    # Poczekaj chwilę, aby strona zdążyła się zaktualizować
    time.sleep(2)

# Zamknij przeglądarkę
driver.quit()

# Pytanie użytkownika, czy chce zobaczyć działające kody PSC
show_codes = input("Czy chcesz zobaczyć działające kody PSC? (Wpisz 'tak' i naciśnij Enter): ")

# Wyświetlanie działających kodów PSC
if show_codes.lower() == 'tak':
    print("\nDziałające kody PSC:")
    for code in generated_codes:
        if check_psc(code):
            print(code)
