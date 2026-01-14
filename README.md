"""
CURSED TERMINAL: THE HACKER'S NIGHTMARE
A Halloween Horror Game by Github : @SoumyaEXE
Programmiz October Challenge 2025
"""

import random
import time
import sys


try:
    from colorama import init, Fore, Back, Style
    init(autoreset=True)
    COLORS_AVAILABLE = True
except ImportError:
    COLORS_AVAILABLE = False

    class Fore:
        RED = GREEN = YELLOW = BLUE = MAGENTA = CYAN = WHITE = RESET = ""
    class Back:
        BLACK = RED = GREEN = YELLOW = BLUE = MAGENTA = CYAN = WHITE = RESET = ""
    class Style:
        BRIGHT = DIM = NORMAL = RESET_ALL = ""


game_state = {
    "sanity": 100,
    "files_recovered": 0,
    "entities_encountered": 0,
    "ending": None
}

def clear_screen():
    """Clear the terminal screen with newlines"""
    print("\n" * 50)

def slow_print(text, delay=0.03, color=Fore.WHITE):
    """Print text with typewriter effect"""
    for char in text:
        sys.stdout.write(color + char)
        sys.stdout.flush()
        time.sleep(delay)
    print(Style.RESET_ALL)

def glitch_text(text, intensity=3):
    """Add glitch effect to text"""
    glitch_chars = "!@#$%^&*()_+-=[]{}|;:,.<>?/~`"
    result = ""
    for char in text:
        if random.randint(1, 10) <= intensity and char != " ":
            result += random.choice(glitch_chars)
        else:
            result += char
    return result

def display_status():
    """Display current game status"""
    sanity_color = Fore.GREEN if game_state["sanity"] > 70 else Fore.YELLOW if game_state["sanity"] > 30 else Fore.RED
    print("\n" + "="*60)
    print(f"{sanity_color}[SANITY: {game_state['sanity']}%] {Fore.CYAN}[FILES: {game_state['files_recovered']}/3] {Fore.MAGENTA}[ENTITIES: {game_state['entities_encountered']}]")
    print("="*60 + "\n")

def decrease_sanity(amount):
    """Decrease player's sanity"""
    game_state["sanity"] = max(0, game_state["sanity"] - amount)
    if game_state["sanity"] <= 0:
        game_over_insanity()

def intro_sequence():
    """Game introduction with atmosphere"""
    clear_screen()
    ascii_skull = f"""{Fore.RED}
                                 ...----....
                         ..-:"''         ''"-..
                      .-'                      '-.
                    .'              .     .       '.
                  .'   .          .    .      .    .''.
                .'  .    .       .   .   .     .   . ..:.
              .' .   . .  .       .   .   ..  .   . ....::.
             ..   .   .      .  .    .     .  ..  . ....:IA.
            .:  .   .    .    .  .  .    .. .  .. .. ....:IA.
           .: .   .   ..   .    .     . . .. . ... ....:.:VHA.
           '..  .  .. .   .       .  . .. . .. . .....:.::IHHB.
          .:. .  . .  . .   .  .  . . . ...:.:... .......:HIHMM.
         .:.... .   . ."::"'.. .   .  . .:.:.:II;,. .. ..:IHIMMA
         ':.:..  ..::IHHHHHI::. . .  ...:.::::.,,,. . ....VIMMHM
        .:::I. .AHHHHHHHHHHAI::. .:...,:IIHHHHHHMMMHHL:. . VMMMM
       .:.:V.:IVHHHHHHHMHMHHH::..:" .:HIHHHHHHHHHHHHHMHHA. .VMMM.
       :..V.:IVHHHHHMMHHHHHHHB... . .:VPHHMHHHMMHHHHHHHHHAI.:VMMI
       ::V..:VIHHHHHHMMMHHHHHH. .   .I":IIMHHMMHHHHHHHHHHHAPI:WMM
       ::". .:.HHHHHHHHMMHHHHHI.  . .:..I:MHMMHHHHHHHHHMHV:':H:WM
       :: . :.::IIHHHHHHMMHHHHV  .ABA.:.:IMHMHMMMHMHHHHV:'. .IHWW
       '.  ..:..:.:IHHHHHMMHV" .AVMHMA.:.'VHMMMMHHHHHV:' .  :IHWV
        :.  .:...:".:.:TPP"   .AVMMHMMA.:. "VMMHHHP.:... .. :IVAI
       .:.   '... .:"'   .   ..HMMMHMMMA::. ."VHHI:::....  .:IHW'
       ...  .  . ..:IIPPIH: ..HMMMI.MMMV:I:.  .:ILLH:.. ...:I:IM
     : .   .'"' .:.V". .. .  :HMMM:IMMMI::I. ..:HHIIPPHI::'.P:HM.
     :.  .  .  .. ..:.. .    :AMMM IMMMM..:...:IV":T::I::.".:IHIMA
     'V:.. .. . .. .  .  .   'VMMV..VMMV :....:V:.:..:....::IHHHMH
       "IHH:.II:.. .:. .  . . . " :HB"" . . ..PI:.::.:::..:IHHMMV"
        :IP""HHII:.  .  .    . . .'V:. . . ..:IH:.:.::IHIHHMMMMM"
        :V:. VIMA:I..  .     .  . .. . .  .:.I:I:..:IHHHHMMHHMMM
        :"VI:.VWMA::. .:      .   .. .:. ..:.I::.:IVHHHMMMHMMMMI
        :."VIIHHMMA:.  .   .   .:  .:.. . .:.II:I:AMMMMMMHMMMMMI
        :..VIHIHMMMI...::.,:.,:!"I:!"I!"I!"V:AI:VAMMMMMMHMMMMMM'
        ':.:HIHIMHHA:"!!"I.:AXXXVVXXXXXXXA:."HPHIMMMMHHMHMMMMMV
          V:H:I:MA:W'I :AXXXIXII:IIIISSSSSSXXA.I.VMMMHMHMMMMMM
            'I::IVA ASSSSXSSSSBBSBMBSSSSSSBBMMMBS.VVMMHIMM'"'
             I:: VPAIMSSSSSSSSSBSSSMMBSSSBBMMMMXXI:MMHIMMI
            .I::. "H:XIIXBBMMMMMMMMMMMMMMMMMBXIXXMMPHIIMM'
            :::I.  ':XSSXXIIIIXSSBMBSSXXXIIIXXSMMAMI:.IMM
            :::I:.  .VSSSSSISISISSSBII:ISSSSBMMB:MI:..:MM
            ::.I:.  ':"SSSSSSSISISSXIIXSSSSBMMB:AHI:..MMM.
            ::.I:. . ..:"BBSSSSSSSSSSSSBBBMMMB:AHHI::.HMMI
            :..::.  . ..::":BBBBBSSBBBMMMB:MMMMHHII::IHHMI
            ':.I:... ....:IHHHHHMMMMMMMMMMMMMMMHHIIIIHMMV"
              "V:. ..:...:.IHHHMMMMMMMMMMMMMMMMHHHMHHMHP'
               ':. .:::.:.::III::IHHHHMMMMMHMHMMHHHHM"
                 "::....::.:::..:..::IIIIIHHHHMMMHHMV"
                   "::.::.. .. .  ...:::IIHHMMMMHMV"
                     "V::... . .I::IHHMMV"'
                       '"VHVHHHAHHHHMMV:"'

    {Style.RESET_ALL}"""
    print(ascii_skull)
    time.sleep(1)
    
    slow_print(f"\n{Fore.CYAN}{'='*60}", delay=0.01)
    slow_print(f"{Fore.RED}{Style.BRIGHT}    CURSED TERMINAL: THE HACKER'S NIGHTMARE", delay=0.05)
    slow_print(f"{Fore.CYAN}{'='*60}\n", delay=0.01)
    
    time.sleep(0.5)
    slow_print(f"{Fore.YELLOW}System Status: {Fore.RED}COMPROMISED")
    slow_print(f"{Fore.YELLOW}User Access: {Fore.RED}RESTRICTED")
    slow_print(f"{Fore.YELLOW}Time Until System Wipe: {Fore.RED}UNKNOWN\n")
    
    time.sleep(0.5)
    slow_print(f"{Fore.WHITE}You wake up at your computer at 3:47 AM.")
    slow_print("Your screen flickers with an eerie green glow.")
    slow_print("The last thing you remember is downloading a 'cracked software'...")
    slow_print(f"\n{Fore.RED}Something is very, very wrong.\n")
    
    input(f"{Fore.CYAN}Press ENTER to continue...")

def corrupted_boot():
    """Display corrupted boot sequence"""
    clear_screen()
    messages = [
        f"{Fore.GREEN}[OK] Starting system services...",
        f"{Fore.GREEN}[OK] Mounting filesystems...",
        f"{Fore.YELLOW}[WARN] Unknown process detected...",
        f"{Fore.RED}[FAIL] Security protocols corrupted",
        f"{Fore.RED}[FAIL] {glitch_text('THEY ARE WATCHING YOU')}",
        f"{Fore.GREEN}[OK] Loading user environment...",
        f"{Fore.RED}[FAIL] {glitch_text('YOU SHOULD NOT HAVE COME HERE')}",
    ]
    
    for msg in messages:
        print(msg)
        time.sleep(0.3)
    
    time.sleep(1)
    slow_print(f"\n{Fore.RED}>>> MALWARE DETECTED: 'PHANTOM.EXE'")
    slow_print(f"{Fore.RED}>>> WARNING: Entity has achieved consciousness")
    slow_print(f"{Fore.RED}>>> Your files are being held hostage\n")
    
    time.sleep(1)
    slow_print(f"{Fore.CYAN}You must recover 3 corrupted files to regain control.")
    slow_print(f"{Fore.CYAN}But be warned... you're not alone in this system.\n")
    
    decrease_sanity(5)
    input(f"{Fore.YELLOW}Press ENTER to access terminal...")

def file_recovery_puzzle():
    """File recovery mini-game"""
    clear_screen()
    display_status()
    
    slow_print(f"{Fore.CYAN}>>> Scanning for recoverable files...")
    time.sleep(1)
    
    filename = random.choice(["memories.dat", "identity.sys", "reality.cfg"])
    slow_print(f"{Fore.GREEN}>>> Found: {filename}")
    
    # Generate encryption key puzzle
    key_length = 4
    correct_key = ''.join([str(random.randint(0, 9)) for _ in range(key_length)])
    
    slow_print(f"\n{Fore.YELLOW}File is encrypted. Decode the key:")
    slow_print(f"{Fore.WHITE}Cipher: Each number is the sum of its adjacent digits")
    
    # Create puzzle
    cipher = []
    for i, digit in enumerate(correct_key):
        val = int(digit)
        if i > 0:
            val += int(correct_key[i-1])
        if i < len(correct_key) - 1:
            val += int(correct_key[i+1])
        cipher.append(str(val))
    
    slow_print(f"{Fore.CYAN}Cipher sequence: {'-'.join(cipher)}")
    
    attempts = 3
    while attempts > 0:
        user_input = input(f"\n{Fore.WHITE}Enter decryption key (or 'skip'): ").strip()
        
        if user_input.lower() == 'skip':
            slow_print(f"{Fore.RED}>>> File recovery failed. The entity grows stronger...")
            decrease_sanity(15)
            return False
        
        if user_input == correct_key:
            slow_print(f"{Fore.GREEN}>>> ACCESS GRANTED! File recovered.")
            game_state["files_recovered"] += 1
            decrease_sanity(5)
            time.sleep(1)
            return True
        else:
            attempts -= 1
            if attempts > 0:
                slow_print(f"{Fore.RED}>>> INCORRECT. {attempts} attempts remaining.")
                decrease_sanity(10)
            else:
                slow_print(f"{Fore.RED}>>> LOCKED OUT. The entity corrupts the file...")
                decrease_sanity(20)
                return False
    
    return False

def entity_encounter():
    """Random encounter with the digital entity"""
    clear_screen()
    display_status()
    
    game_state["entities_encountered"] += 1
    
    encounters = [
        {
            "desc": f"{Fore.RED}Your screen fills with static. Through the noise, you see EYES staring at you.",
            "choices": ["Look away", "Stare back", "Shut down monitor"],
            "outcomes": [
                ("You avert your gaze. The entity loses interest... for now.", 5),
                ("You lock eyes with the entity. It pierces into your soul.", 25),
                ("The monitor won't turn off. The eyes multiply.", 15)
            ]
        },
        {
            "desc": f"{Fore.RED}Text begins typing on its own: 'I KNOW WHERE YOU LIVE'",
            "choices": ["Delete the text", "Reply 'No you don't'", "Ignore it"],
            "outcomes": [
                ("You delete it but your address appears on screen.", 20),
                ("It responds with your exact coordinates. Terror grips you.", 25),
                ("The text multiplies, filling your screen with your personal info.", 15)
            ]
        },
        {
            "desc": f"{Fore.RED}All your files start renaming to 'HELP_ME.txt'. A voice whispers from your speakers.",
            "choices": ["Open a file", "Mute speakers", "Pull the power cord"],
            "outcomes": [
                ("The file contains a distorted image of your own face screaming.", 20),
                ("The whispers get louder despite being muted.", 15),
                ("Your computer is still running. It's too late to escape.", 10)
            ]
        }
    ]
    
    encounter = random.choice(encounters)
    slow_print(f"\n{encounter['desc']}\n")
    decrease_sanity(5)
    time.sleep(1)
    
    print(f"{Fore.YELLOW}What do you do?")
    for i, choice in enumerate(encounter['choices'], 1):
        print(f"{Fore.WHITE}{i}. {choice}")
    
    while True:
        try:
            choice = int(input(f"\n{Fore.CYAN}Your choice (1-3): "))
            if 1 <= choice <= 3:
                break
        except ValueError:
            pass
        print(f"{Fore.RED}Invalid choice. Try again.")
    
    outcome, sanity_loss = encounter['outcomes'][choice - 1]
    slow_print(f"\n{Fore.WHITE}{outcome}")
    decrease_sanity(sanity_loss)
    
    time.sleep(2)
    input(f"\n{Fore.YELLOW}Press ENTER to continue...")

def final_confrontation():
    """Final boss encounter"""
    clear_screen()
    
    ascii_demon = f"""{Fore.RED}
    ⠀⠀⠀⠀                      :::!~!!!!!:.
                  .xUHWH!! !!?M88WHX:.
                .X*#M@$!!  !X!M$$$$$$WWx:.
               :!!!!!!?H! :!$!$$$$$$$$$$8X:
              !!~  ~:~!! :~!$!#$$$$$$$$$$8X:
             :!~::!H!&lt;   ~.U$X!?R$$$$$$$$MM!
             ~!~!!!!~~ .:XW$$$U!!?$$$$$$RMM!
               !:~~~ .:!M"T#$$$$WX??#MRRMMM!
               ~?WuxiW*`   `"#$$$$8!!!!??!!!
             :X- M$$$$       `"T#$T~!8$WUXU~
            :%`  ~#$$$m:        ~!~ ?$$$$$$
          :!`.-   ~T$$$$8xx.  .xWW- ~""##*"
.....   -~~:&lt;` !    ~?T#$$@@W@*?$$      /`
W$@@M!!! .!~~ !!     .:XUW$W!~ `"~:    :
#"~~`.:x%`!!  !H:   !WM$$$$Ti.: .!WUn+!`
:::~:!!`:X~ .: ?H.!u "$$$B$$$!W:U!T$$M~
.~~   :X@!.-~   ?@WTWo("*$$$W$TH$! `
Wi.~!X$?!-~    : ?$$$B$Wu("**$RM!
$R@i.~~ !     :   ~$$$$$B$$en:``
?MXT@Wx.~    :     ~"##*$$$$M~⠀⠀⠀⠀⠀⠀
    {Style.RESET_ALL}"""
    
    print(ascii_demon)
    slow_print(f"\n{Fore.RED}{Style.BRIGHT}The entity manifests before you.\n", delay=0.05)
    
    time.sleep(1)
    slow_print(f"{Fore.MAGENTA}PHANTOM: 'You've done well to make it this far, human.'")
    slow_print(f"{Fore.MAGENTA}PHANTOM: 'But your files... your memories... they belong to ME now.'")
    slow_print(f"{Fore.MAGENTA}PHANTOM: 'I offer you a choice...'\n")
    
    time.sleep(1)
    print(f"{Fore.YELLOW}1. Fight the entity (Attempt system purge)")
    print(f"{Fore.YELLOW}2. Make a deal (Sacrifice something)")
    print(f"{Fore.YELLOW}3. Surrender (Accept your fate)")
    
    choice = input(f"\n{Fore.CYAN}Your final decision: ").strip()
    
    if choice == "1":
        ending_fight()
    elif choice == "2":
        ending_deal()
    else:
        ending_surrender()

def ending_fight():
    """Fight ending"""
    clear_screen()
    slow_print(f"{Fore.CYAN}>>> Initiating emergency system purge...")
    slow_print(f"{Fore.CYAN}>>> Loading anti-malware protocols...")
    
    time.sleep(1)
    
    if game_state["files_recovered"] >= 2 and game_state["sanity"] >= 40:
        slow_print(f"\n{Fore.GREEN}>>> SUCCESS! The entity is being purged!")
        slow_print(f"{Fore.RED}PHANTOM: 'NOOOOOOOO! THIS ISN'T OVER!'")
        slow_print(f"\n{Fore.WHITE}The screen goes dark. When it lights up again, everything is normal.")
        slow_print(f"Your files are restored. But you can never forget what lurked in your machine...")
        slow_print(f"\n{Fore.GREEN}{Style.BRIGHT}*** ENDING: LIBERATION ***")
        game_state["ending"] = "LIBERATION"
    else:
        slow_print(f"\n{Fore.RED}>>> FAILED! Not enough resources!")
        slow_print(f"{Fore.RED}PHANTOM: 'You're too weak! Your system is MINE!'")
        slow_print(f"\n{Fore.WHITE}Your screen goes black. When you try to restart...")
        slow_print(f"Every file is corrupted. Every photo shows HIS face.")
        slow_print(f"You are trapped forever in the cursed terminal.")
        slow_print(f"\n{Fore.RED}{Style.BRIGHT}*** ENDING: CONSUMED ***")
        game_state["ending"] = "CONSUMED"

def ending_deal():
    """Deal ending"""
    clear_screen()
    slow_print(f"{Fore.MAGENTA}PHANTOM: 'Wise choice. I need a new host...'")
    slow_print(f"\n{Fore.WHITE}You feel yourself being pulled into the screen.")
    slow_print(f"Your consciousness merges with the entity.")
    slow_print(f"You become the very thing you feared.")
    slow_print(f"\nSomeone else will download infected software soon...")
    slow_print(f"And YOU will be waiting for them.")
    slow_print(f"\n{Fore.MAGENTA}{Style.BRIGHT}*** ENDING: FUSION ***")
    game_state["ending"] = "FUSION"

def ending_surrender():
    """Surrender ending"""
    clear_screen()
    slow_print(f"{Fore.RED}You close your eyes and accept your fate.")
    slow_print(f"\n{Fore.MAGENTA}PHANTOM: 'A pity. I expected more fight.'")
    slow_print(f"\n{Fore.WHITE}Your computer shuts down. When you try to turn it on...")
    slow_print(f"Nothing. Complete silence.")
    slow_print(f"Your digital life is erased. No photos. No memories. Nothing.")
    slow_print(f"You are free, but at what cost?")
    slow_print(f"\n{Fore.YELLOW}{Style.BRIGHT}*** ENDING: ERASURE ***")
    game_state["ending"] = "ERASURE"

def game_over_insanity():
    """Game over due to sanity loss"""
    clear_screen()
    slow_print(f"{Fore.RED}Your sanity has shattered completely.")
    slow_print(f"\n{Fore.WHITE}You can no longer distinguish reality from the digital nightmare.")
    slow_print(f"Your hands move on their own, typing commands you don't understand.")
    slow_print(f"You've become a puppet of the entity.")
    slow_print(f"\n{Fore.RED}{Style.BRIGHT}*** ENDING: BROKEN MIND ***")
    game_state["ending"] = "BROKEN"
    show_final_stats()
    sys.exit()

def show_final_stats():
    """Display final game statistics"""
    print(f"\n{Fore.CYAN}{'='*60}")
    print(f"{Fore.YELLOW}FINAL STATISTICS:")
    print(f"{Fore.WHITE}Final Sanity: {game_state['sanity']}%")
    print(f"{Fore.WHITE}Files Recovered: {game_state['files_recovered']}/3")
    print(f"{Fore.WHITE}Entity Encounters: {game_state['entities_encountered']}")
    print(f"{Fore.WHITE}Ending: {game_state['ending']}")
    print(f"{Fore.CYAN}{'='*60}\n")
    
    slow_print(f"{Fore.GREEN}Thanks for playing CURSED TERMINAL!")
    slow_print(f"{Fore.YELLOW}Sweet dreams... if you can still sleep.\n")

def main_game_loop():
    """Main game loop"""
    while game_state["files_recovered"] < 3:
        if random.random() < 0.4:  # 40% chance of encounter
            entity_encounter()
        
        if file_recovery_puzzle():
            slow_print(f"{Fore.GREEN}Progress saved. Files recovered: {game_state['files_recovered']}/3")
            time.sleep(1)
        
        if game_state["files_recovered"] >= 3:
            break
        
        if game_state["sanity"] <= 0:
            game_over_insanity()
    
    # If player recovered all files, final confrontation
    if game_state["files_recovered"] >= 3:
        slow_print(f"\n{Fore.GREEN}All files recovered! But the entity blocks your exit...")
        time.sleep(2)
        final_confrontation()

def main():
    """Main game function"""
    intro_sequence()
    corrupted_boot()
    main_game_loop()
    show_final_stats()

if __name__ == "__main__":
    main()
