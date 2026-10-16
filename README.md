# jonard_database-to-XAMPP MAO NI EXPLAINATION SA CODE SIR HEHE
# ============================================================
# DUPLICATE / EXPLAINED VERSION ng database.py
# Pareho lang ang code sa database.py, pero may paliwanag
# sa bawat linya para maintindihan kung ano ang nangyayari.
# ============================================================

import os  # Built-in na module para basahin ang environment variables (galing sa .env).

import mysql.connector  # Driver/library na nagko-connect sa MySQL/MariaDB.
from mysql.connector import Error  # Error class para sa try/except.

try:  # Simula ng "subukan ito" block. Kung may mag-fail, eh di laktawan.
    from dotenv import load_dotenv  # Basahin ang .env file.
    load_dotenv()  # Binabasa ang .env file at ginagawang environment variables (hal. DB_HOST, DB_USER).
except ImportError:  # Kung hindi naka-install ang python-dotenv (wala ang module)...
    pass  # ...eh di "pass" lang — hindi mag-e-error, tuloy lang ang program gamit ang default values.


# Ito ang dictionary (config) na naglalaman ng connection details papuntang MySQL.
DB_CONFIG = {
    "host": os.getenv("DB_HOST", "localhost"),  # Host/kung saan naka-host ang DB. Default: localhost (sariling PC).
    "port": int(os.getenv("DB_PORT", "3306")),  # Port ng MySQL. Default: 3306 (XAMPP). int() para gawing number.
    "user": os.getenv("DB_USER", "root"),  # Username ng MySQL. Default: root (default ng XAMPP).
    "password": os.getenv("DB_PASSWORD", ""),  # Password ng MySQL. Default: "" (walang password ang XAMPP).
    "database": os.getenv("DB_NAME", "portfolio_db"),  # Pangalan ng database. Default: portfolio_db.
}


def get_connection():  # Function na gumagawa at nagbabalik ng bagong koneksyon sa MySQL.
    try:  # Subukan ang sumusunod na code...
        connection = mysql.connector.connect(**DB_CONFIG)  # Kumokonekta gamit ang DB_CONFIG. Ang ** ay "unpack" ng dictionary.
        if connection.is_connected():  # Tinitignan kung buhay/tagumpay ba talaga ang koneksyon.
            return connection  # Kung buhay, ibalik yung connection object (o "plug" papuntang DB).
    except Error as e:  # Kung may error habang kumokonekta (mali password, patay ang MySQL, atbp.)...
        print(f"[DB] Connection error: {e}")  # I-print ang error message. Ang {e} ay ang detalye ng error.
        return None  # Ibalik ang None para malaman ng caller na bigo ang koneksyon.


def test_connection():  # Function na pang-test kung gumagana ba ang koneksyon.
    connection = get_connection()  # Tinatawag yung get_connection() para subukan kumonekta.
    if connection:  # Kung may laman (hindi None) — ibig sabihin success ang koneksyon.
        print("[DB] Connected to MySQL successfully na bossing ko wewew.")  # Success message.
        connection.close()  # Isara ang koneksyon para hindi ito tumagal nang bukas (mabuting gawi).
        return True  # Ibalik ang True = tagumpay.
    print("[DB] Failed to connect oww shiss. Make sure ahh klaruha2 nga MySQL is naka start ba or running in XAMPP.")  # Kapag bigo — bilin na i-start ang MySQL sa XAMPP.
    return False  # Ibalik ang False = bigo.


if __name__ == "__main__":  # Tumatakbo lang ito KUNG direktang ni-run mo yung file (hal. `python database_explained.py`).
    test_connection()  # Kapag direktang ni-run, awtomatikong tatawagin yung test_connection().

