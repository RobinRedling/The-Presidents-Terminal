import time
import os
import random
import json

game_name = "--- The Presidents Terminal – Eat or Be Eaten ---"
table_width = 80

player_stats = {
    "name": "UNBEKANNT",
    "soldaten": 1,
    "gold": 0,
    "safe_gold": 0,
    "moral": 100,
    "spawnrate": 1,
    "rekrutier_zeit": 30,
    "timer": 0,
    "min_zeit": 5,
    "gold_pro_soldat": 0.1,
    "upgrades_anzahl": 0,
    "upgrades_speed": 0,
    "gefallene_soldaten_total": 0,
    "rekrutierte_soldaten_total": 1,
    "max_soldaten_record": 1,
    "besiegte_gegner_total": 0,
    "gewonnene_schlachten": 0,
    "verlorene_schlachten": 0,
    "desertierte_soldaten_total": 0,
    "gesammeltes_gold_total": 0,
    "ausgegebenes_gold_total": 0,
    "safe_einzahlungen_total": 0,
    "safe_auszahlungen_total": 0,
    "lifestyle_ausgaben_total": 0,
    "start_zeit": time.time(),
    "besitz": []
}

SPEED_STEPS = [30, 25, 20, 15, 12, 10, 9, 8, 7, 6, 5]

LIFESTYLE_SHOP = [
    {"name": "Goldene Uhr", "preis": 5000},
    {"name": "Luxusauto", "preis": 50000},
    {"name": "Privatjet", "preis": 500000},
    {"name": "Präsidentenvilla", "preis": 2000000},
    {"name": "Eigene Insel", "preis": 10000000}
]

TEXTS = {
    "sieg": ["SIEG! {n} erobert! Verluste: -{v} Soldaten.", "Erfolg in {n}! Die Flagge weht. (-{v} Mann)"],
    "niederlage": ["Niederlage in {n}! {k} Feinde tot.", "Rückzug aus {n}! Wir wurden überrannt."],
    "nachschub": ["Verstärkung: +{s} Soldaten eingetroffen.", "Frische Truppen an der Front! (+{s})"],
    "desertion": ["Truppen fliehen aufgrund niedriger Moral! (-{v})", "Desertion im Lager gemeldet!"],
    "regeneration": ["Der Feind in {n} hat Truppen regeneriert.", "{n} hat die Verteidigung verstärkt."],
    "safe_in": ["{v} Gold im Präsidenten-Safe gesichert.", "Safe-Einzahlung: +{v} Gold."],
    "safe_out": ["{v} Gold aus dem Safe entnommen.", "Safe-Auszahlung: {v} Gold."],
    "shop_kauf": ["Präsident kaufte {i} für {p} Gold.", "Luxusgut erworben: {i}."],
    "speichern": ["Spielstand erfolgreich gesichert.", "Daten lokal gespeichert."],
    "laden": ["Guten Tag Herr Präsident. Willkommen zurück!", "Willkommen zurück, Präsident."],
    "moral_boost": ["Moral durch Präsidentschafts-Dekret erhöht!", "Truppen sind motiviert! (+20% Moral)"],
    "upgrade_anzahl": ["Ausbau abgeschlossen: +{n} Rekrutierungs-Kapazität!", "Neue Kasernen errichtet! (+{n} Soldaten)"],
    "upgrade_speed": ["Ausbildung beschleunigt! Neue Zeit: {t}s!", "Effizienz gesteigert: Rekrutierung dauert nur noch {t}s."],
    "error_gold": ["Finanzstopp! Nicht genug Gold für diese Aktion.", "Bank meldet: Guthaben reicht nicht aus!", "Konto unterdeckt! Wir brauchen mehr Gold."],
    "error_max": ["Limit erreicht! Mehr ist aktuell nicht möglich.", "Maximale Effizienz bereits erreicht!", "Technisches Limit: Stufe ist bereits auf Maximum."], 
    "error_owned": ["Präsident, das besitzen Sie bereits!", "Dieses Luxusgut befindet sich schon in Ihrem Besitz."]
}

regionen = [
    {"name": "Norden", "gegner": 100, "max_gegner": 100, "besitz": False, "bonus": 1, "reset_at": 0},
    {"name": "Nord-Osten", "gegner": 1000, "max_gegner": 1000, "besitz": False, "bonus": 10, "reset_at": 0},
    {"name": "Osten", "gegner": 1000, "max_gegner": 1000, "besitz": False, "bonus": 10, "reset_at": 0},
    {"name": "Süd-Osten", "gegner": 1000, "max_gegner": 1000, "besitz": False, "bonus": 10, "reset_at": 0},
    {"name": "Süden", "gegner": 10000, "max_gegner": 10000, "besitz": False, "bonus": 100, "reset_at": 0},
    {"name": "Süd-Westen", "gegner": 1000000, "max_gegner": 1000000, "besitz": False, "bonus": 10000, "reset_at": 0},
    {"name": "Westen", "gegner": 2000000, "max_gegner": 2000000, "besitz": False, "bonus": 20000, "reset_at": 0},
    {"name": "Nord-Westen", "gegner": 10000000, "max_gegner": 10000000, "besitz": False, "bonus": 100000, "reset_at": 0},
]

event_log = ["Angriff ist die beste Verteidigung!"]
max_events = 8 

def clear_screen():
    os.system('cls' if os.name == 'nt' else 'clear')

def draw_line(char="="):
    print(char * table_width)

def format_center(text):
    return f"| {str(text).center(table_width - 4)} |"

def format_left(text):
    return f"| {str(text).ljust(table_width - 4)} |"

def format_menu_item(key, label, info, cost_text):
    col1 = f"{key}. {label}".ljust(31)
    col2 = f"({info})".ljust(19)
    col3 = f"Kosten: {cost_text}".ljust(26)
    return f"| {col1}{col2}{col3} |"

def add_event_dict(key, **kwargs):
    raw_text = random.choice(TEXTS[key])
    event_log.append(raw_text.format(**kwargs))
    while len(event_log) > max_events: event_log.pop(0)

def get_income_per_sec():
    basis = player_stats["soldaten"] * player_stats["gold_pro_soldat"]
    bonus = sum(r["bonus"] for r in regionen if r["besitz"])
    return basis + bonus

def save_game():
    data = {"stats": player_stats, "regions": regionen}
    with open("savegame.json", "w") as f:
        json.dump(data, f)
    add_event_dict("speichern")

def load_game():
    if not os.path.exists("savegame.json"): return False
    try:
        with open("savegame.json", "r") as f:
            data = json.load(f)
            player_stats.update(data["stats"])
            for i, r_data in enumerate(data["regions"]):
                if i < len(regionen): regionen[i].update(r_data)
            add_event_dict("laden")
            return True
    except: return False

def kaempfen(region):
    if region["besitz"]: 
        return
    
    if player_stats["soldaten"] >= region["gegner"]:
        v = int(region["gegner"] * (0.4 * (1.2 - (player_stats["moral"] / 100))))
        player_stats["besiegte_gegner_total"] += region["gegner"]
        player_stats["gefallene_soldaten_total"] += v
        player_stats["soldaten"] = max(0, player_stats["soldaten"] - v)
        player_stats["gewonnene_schlachten"] += 1
        region["besitz"] = True
        region["gegner"] = 0
        add_event_dict("sieg", n=region["name"], v=v)
        player_stats["moral"] = min(100, player_stats["moral"] + 5)

    else:
        k = min(int(player_stats["soldaten"] * (0.5 + (player_stats["moral"] / 200))), region["gegner"])
        v_e = int(player_stats["soldaten"] * 0.9)
        player_stats["gefallene_soldaten_total"] += v_e
        player_stats["soldaten"] = max(0, player_stats["soldaten"] - v_e)
        player_stats["verlorene_schlachten"] += 1
        region["gegner"] -= k
        region["reset_at"] = time.time() + 3600
        player_stats["moral"] = max(0, player_stats["moral"] - 25)
        add_event_dict("niederlage", n=region["name"], k=k)

def start_screen():
    clear_screen()
    title_ascii = r"""
    
 _____ _              ___               _     _            _       
/__   \ |__   ___    / _ \_ __ ___  ___(_) __| | ___ _ __ | |_ ___ 
  / /\/ '_ \ / _ \  / /_)/ '__/ _ \/ __| |/ _` |/ _ \ '_ \| __/ __|
 / /  | | | |  __/ / ___/| | |  __/\__ \ | (_| |  __/ | | | |_\__ \
 \/   |_| |_|\___| \/    |_|  \___||___/_|\__,_|\___|_| |_|\__|___/
                                                                   
             _____                    _             _              
            /__   \___ _ __ _ __ ___ (_)_ __   __ _| |             
              / /\/ _ \ '__| '_ ` _ \| | '_ \ / _` | |             
             / / |  __/ |  | | | | | | | | | | (_| | |             
             \/   \___|_|  |_| |_| |_|_|_| |_|\__,_|_|             
                                                                   
    """
    
    draw_line("#")
    print(format_center(game_name))
    draw_line("#")
    
    for line in title_ascii.split("\n"):
        if line:
            print(line.center(table_width))
    
    draw_line("#")
    print(format_center("Drücke ENTER ZUM INITIALISIEREN"))
    draw_line("#")
    
    input()

def init_game():
    clear_screen()
    
    draw_line("#")
    print(format_center("SYSTEM-INITIALISIERUNG"))
    draw_line("#")

    boot_messages = [
        "Lade ballistische Berechnungsmodule...",
        "Synchronisiere Satelliten-Netzwerk...",
        "Prüfe Goldreserven und Munitionsbestände...",
        "Autorisiere Präsidentschafts-Zugang..."
    ]
    
    for msg in boot_messages:
        print(format_left(f" [ OK ] {msg}"))
        time.sleep(0.4)
    
    draw_line("=")

    if os.path.exists("savegame.json"):
        print(format_center("Die Tür zu Ihrem Büro ist geöffnet, Herr Präsident."))
        draw_line("#")
        
        wahl = input("\n" + " " * 25 + "> Spielstand laden? (j/n): ").lower()
        
        if wahl == "j":
            if load_game():
                clear_screen()
                draw_line("#")
                print(format_center("Betrete Präsidentenamt. WILLKOMMEN ZURÜCK!"))
                draw_line("#")
                time.sleep(2)
                return

    clear_screen()
    draw_line("#")
    print(format_center("Neue Amtszeit"))
    draw_line("#")
    print("\n")
    print(format_center("Wie lautet ihr Name, Herr Präsident:"))
    
    name = input("\n" + " " * 25 + "> Name: ").upper()
    player_stats["name"] = name if name.strip() != "" else "ohne Namen!"
    
    clear_screen()
    draw_line("#")
    print(format_center(f"Viel Erfolg, Präsident {player_stats['name']}!"))
    draw_line("#")
    time.sleep(2)

start_screen()
init_game()

last_time = time.time()
menu = "haupt"
aktion = ""

while True:
    now = time.time()
    dt, last_time = now - last_time, now

    for r in regionen:
        if not r["besitz"] and r["gegner"] < r["max_gegner"] and r["reset_at"] != 0:
            if now >= r["reset_at"]:
                r["gegner"], r["reset_at"] = r["max_gegner"], 0
                add_event_dict("regeneration", n=r["name"])

    income_ps = get_income_per_sec()
    gain = income_ps * dt
    player_stats["gold"] += gain
    player_stats["gesammeltes_gold_total"] += gain
    player_stats["moral"] = max(0, player_stats["moral"] - 0.02 * dt)

    player_stats["timer"] += dt
    while player_stats["timer"] >= player_stats["rekrutier_zeit"]:
        player_stats["soldaten"] += player_stats["spawnrate"]
        player_stats["rekrutierte_soldaten_total"] += player_stats["spawnrate"]
        player_stats["timer"] -= player_stats["rekrutier_zeit"]
        add_event_dict("nachschub", s=player_stats['spawnrate'])

    if player_stats["moral"] < 15:
        verl_rate = player_stats["soldaten"] * 0.01 * dt
        if verl_rate > 0.5:
            player_stats["soldaten"] = max(0, player_stats["soldaten"] - verl_rate)
            player_stats["desertierte_soldaten_total"] += verl_rate
            if random.random() < 0.1 and player_stats["soldaten"] > 0:
                add_event_dict("desertion", v=max(1, int(verl_rate)))

    kosten_anzahl = (player_stats["upgrades_anzahl"] + 1) * 10
    kosten_speed = (player_stats["upgrades_speed"] + 1) * 15
    kosten_moral = max(500, int(player_stats["soldaten"] * 5))
    amt_sek = int(time.time() - player_stats["start_zeit"])
    sek_bis_spawn = int(max(0, player_stats["rekrutier_zeit"] - player_stats["timer"]))

    clear_screen()
    draw_line("#")
    print(format_center(game_name))
    print(format_center(f"Präsident: {player_stats['name']}"))
    draw_line("#")
    print(format_center(f"Soldaten: {int(player_stats['soldaten'])} | Gold: {int(player_stats['gold'])} | Safe: {int(player_stats['safe_gold'])} | Moral: {int(player_stats['moral'])}%"))
    print(format_center(f"+{player_stats['spawnrate']} Soldaten in {sek_bis_spawn}s | alle {int(player_stats['rekrutier_zeit'])}s"))
    print(format_center(f"Einkommen: {int(income_ps * 60)} Gold/Min | Fortschritt: {sum(1 for r in regionen if r['besitz'])}/8 Regionen"))
    draw_line("=")

    if menu == "haupt":
        print(format_menu_item("1", "Rekruten +", f"Stufe {player_stats['upgrades_anzahl']}", f"{kosten_anzahl} Gold"))
        is_max = player_stats["upgrades_speed"] >= len(SPEED_STEPS) - 1
        print(format_menu_item("2", "Ausbildung +", "MAX" if is_max else f"Stufe {player_stats['upgrades_speed']}", "-" if is_max else f"{kosten_speed} Gold"))
        print(format_menu_item("3", "Moral-Boost", "+20%", f"{kosten_moral} Gold"))
        print(format_menu_item("4", "Landkarte", "Regionen", "-"))
        print(format_menu_item("5", "Kriegsarchiv", "Statistiken", "-"))
        print(format_menu_item("6", "Präsidentenamt", "Lifestyle", "-"))
        print(format_menu_item("7", "Speichern", "Spielstand", "-"))
        draw_line("-")
        print(format_center("TIPP: Um Upgrades schneller zu kaufen, nutze 11 oder 111"))

    elif menu == "praesident":
        print(format_center("--- PRÄSIDENTENAMT ---"))
        print(format_left(f" Zeit im Amt: {amt_sek // 3600}h {(amt_sek % 3600) // 60}m {amt_sek % 60}s"))
        print(format_left(f" Besitz: {', '.join(player_stats['besitz']) if player_stats['besitz'] else 'Kein Besitz'}"))
        print(format_left("-" * 76))
        print(format_menu_item("1", "Kasse", "Ein/Auszahlung", "-"))
        print(format_menu_item("2", "Lifestyle-Shop", "Luxusgüter", "-"))
        print(format_left(" 0. Zurück"))

    elif menu == "kasse":
        print(format_center("--- KASSE ---"))
        print(format_center(f"Bargeld: {int(player_stats['gold'])} | Safe: {int(player_stats['safe_gold'])}"))
        print(format_left(" EINZAHLEN: 1(10%) | 2(25%) | 3(50%) | 4(75%) | 5(100%)"))
        print(format_left(" AUSZAHLEN: 6(10%) | 7(25%) | 8(50%) | 9(75%) | 10(100%)"))
        print(format_left(" 0. Zurück"))

    elif menu == "shop":
        print(format_center("--- LIFESTYLE-SHOP ---"))
        for i, item in enumerate(LIFESTYLE_SHOP, 1):
            status = "GEKAUFT" if item["name"] in player_stats["besitz"] else f"{item['preis']} Gold"
            print(format_left(f" {i}. {item['name'].ljust(20)} | {status}"))
        print(format_left(" 0. Zurück"))

    elif menu == "regionen":
        print(format_center("--- LANDKARTE ---"))
        for i, r in enumerate(regionen, 1):
            s = "BESETZT" if r["besitz"] else f"Gegner: {int(r['gegner'])}"
            print(format_left(f"{i}. [{'X' if r['besitz'] else ' '}] {r['name'].ljust(15)} | {s.ljust(25)} | +{r['bonus']} G/s"))
        print(format_left(" 0. Zurück"))

    elif menu == "stats":
        kd = player_stats["besiegte_gegner_total"] / max(1, player_stats["gefallene_soldaten_total"])
        print(format_center("--- KRIEGESARCHIV ---"))
        print(format_left(f" Soldaten Rekrutiert:      {int(player_stats['rekrutierte_soldaten_total'])}"))
        print(format_left(f" Gefallene Soldaten:       {int(player_stats['gefallene_soldaten_total'])}"))
        print(format_left(f" Desertierte Soldaten:     {int(player_stats['desertierte_soldaten_total'])}"))
        print(format_left(f" Besiegte Gegner:          {int(player_stats['besiegte_gegner_total'])}"))
        print(format_left(f" Schlachten (G/V):         {player_stats['gewonnene_schlachten']} / {player_stats['verlorene_schlachten']}"))
        print(format_left(f" K/D Rate:                 {kd:.2f}"))
        print(format_left("-" * 40))
        print(format_left(f" Gold Gesamt verdient:     {int(player_stats['gesammeltes_gold_total'])}"))
        print(format_left(f" Gold Gesamt ausgegeben:   {int(player_stats['ausgegebenes_gold_total'])}"))
        print(format_left(f" Safe-Einzahlungen total:  {int(player_stats['safe_einzahlungen_total'])}"))
        print(format_left(f" Safe-Auszahlungen total:  {int(player_stats['safe_auszahlungen_total'])}"))
        print(format_left(f" Lifestyle Ausgaben:       {int(player_stats['lifestyle_ausgaben_total'])}"))
        print(format_left("-" * 40))
        print(format_left(f" Aktuelle Spawnrate:       +{player_stats['spawnrate']} pro {player_stats['rekrutier_zeit']}s"))
        print(format_left(" 0. Zurück"))

    draw_line("=")
    for event in event_log: 
        print(format_left(f"> {event}"))
    draw_line("=")

    aktion = input("Enter für Refresh | Wahl: ").lower()

    if menu == "haupt":
        if aktion in ["1","11","111","1x"]:
            limit = player_stats["gold"] / (2 if aktion=="11" else 1)
            count = 0
            while player_stats["gold"] >= kosten_anzahl:
                if aktion not in ["111","1x"] and player_stats["gold"] <= (player_stats["gold"]+gain-limit): break
                player_stats["gold"] -= kosten_anzahl
                player_stats["ausgegebenes_gold_total"] += kosten_anzahl
                player_stats["upgrades_anzahl"] += 1
                player_stats["spawnrate"] += 1
                kosten_anzahl = (player_stats["upgrades_anzahl"] + 1) * 10
                count += 1
                if aktion == "1": break
            if count > 0: add_event_dict("upgrade_anzahl", n=count)
            else:
                    add_event_dict("error_gold")

        elif aktion in ["2","22","222","2x"]:
            if player_stats["upgrades_speed"] >= len(SPEED_STEPS) - 1:
                add_event_dict("error_max")
            
            else:
                limit = player_stats["gold"] / (2 if aktion=="22" else 1)
                count = 0
                while player_stats["gold"] >= kosten_speed and player_stats["upgrades_speed"] < len(SPEED_STEPS)-1:
                    if aktion not in ["222","2x"] and player_stats["gold"] <= (player_stats["gold"]+gain-limit): break
                    player_stats["gold"] -= kosten_speed
                    player_stats["ausgegebenes_gold_total"] += kosten_speed
                    player_stats["upgrades_speed"] += 1
                    player_stats["rekrutier_zeit"] = SPEED_STEPS[player_stats["upgrades_speed"]]
                    kosten_speed = (player_stats["upgrades_speed"] + 1) * 15
                    count += 1
                    if aktion == "2": break
                
                if count > 0: 
                    add_event_dict("upgrade_speed", t=player_stats["rekrutier_zeit"])
                else:
                    add_event_dict("error_gold")

        elif aktion == "3" and player_stats["gold"] >= kosten_moral:
            player_stats["gold"] -= kosten_moral
            player_stats["ausgegebenes_gold_total"] += kosten_moral
            player_stats["moral"] = min(100, player_stats["moral"] + 20)
            add_event_dict("moral_boost")
        elif aktion == "4": menu = "regionen"
        elif aktion == "5": menu = "stats"
        elif aktion == "6": menu = "praesident"
        elif aktion == "7": save_game()

    elif menu == "praesident":
        if aktion == "0": menu = "haupt"
        elif aktion == "1": menu = "kasse"
        elif aktion == "2": menu = "shop"

    elif menu == "kasse":
        p = {"1":0.1, "2":0.25, "3":0.5, "4":0.75, "5":1.0, "6":0.1, "7":0.25, "8":0.5, "9":0.75, "10":1.0}
        if aktion == "0": menu = "praesident"
        elif aktion in p:
            if aktion in ["1","2","3","4","5"]:
                v = int(player_stats["gold"] * p[aktion])
                player_stats["gold"] -= v
                player_stats["safe_gold"] += v
                player_stats["safe_einzahlungen_total"] += v
                add_event_dict("safe_in", v=v)
            else:
                v = int(player_stats["safe_gold"] * p[aktion])
                player_stats["safe_gold"] -= v
                player_stats["gold"] += v
                player_stats["safe_auszahlungen_total"] += v
                add_event_dict("safe_out", v=v)

    elif menu == "shop":
        if aktion == "0": 
            menu = "praesident"
        elif aktion.isdigit() and 1 <= int(aktion) <= len(LIFESTYLE_SHOP):
            it = LIFESTYLE_SHOP[int(aktion)-1]
            
            if it["name"] in player_stats["besitz"]:
                add_event_dict("error_owned") 
                
            elif player_stats["gold"] >= it["preis"]:
                player_stats["gold"] -= it["preis"]
                player_stats["ausgegebenes_gold_total"] += it["preis"]
                player_stats["lifestyle_ausgaben_total"] += it["preis"]
                player_stats["besitz"].append(it["name"])
                add_event_dict("shop_kauf", i=it["name"], p=it["preis"])
            
            else:
                add_event_dict("error_gold")

    elif menu == "regionen":
        if aktion == "0": menu = "haupt"
        elif aktion.isdigit() and 1 <= int(aktion) <= len(regionen):
            kaempfen(regionen[int(aktion)-1])
            input("Bestätigen mit ENTER...")

    elif menu == "stats" and aktion == "0": menu = "haupt"
