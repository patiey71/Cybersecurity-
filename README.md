using System;
using System.Collections.Generic;
using System.Threading;

namespace CybersecurityChatbot
{
    // ==================== MAIN PROGRAM ====================
    class Program
    {
        static void Main(string[] args)
        {
            // Display ASCII art header
            ConsoleUI.DisplayHeader();

            // Start the chatbot
            ChatBot bot = new ChatBot();
            bot.Start();
        }
    }

    // ==================== ASCII ART ====================
    public class AsciiArt
    {
        public static string GetLogo()
        {
            return @"
  ██████╗██╗   ██╗██████╗ ███████╗██████╗ 
 ██╔════╝╚██╗ ██╔╝██╔══██╗██╔════╝██╔══██╗
 ██║      ╚████╔╝ ██████╔╝█████╗  ██████╔╝
 ██║       ╚██╔╝  ██╔══██╗██╔══╝  ██╔══██╗
 ╚██████╗   ██║   ██████╔╝███████╗██║  ██║
  ╚═════╝   ╚═╝   ╚═════╝ ╚══════╝╚═╝  ╚═╝
   🔐 Cybersecurity Awareness Assistant 🔐
            ";
        }
    }

    // ==================== CONSOLE UI ====================
    public class ConsoleUI
    {
        public static void DisplayHeader()
        {
            Console.Clear();
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine(AsciiArt.GetLogo());
            Console.ResetColor();
            PrintDivider();
            TypeEffect("  Welcome to the Cybersecurity Awareness Chatbot!", ConsoleColor.Green);
            TypeEffect("  Protecting citizens one conversation at a time.", ConsoleColor.Green);
            PrintDivider();
            Console.WriteLine();
        }

        public static void PrintDivider()
        {
            Console.ForegroundColor = ConsoleColor.DarkCyan;
            Console.WriteLine("  " + new string('═', 55));
            Console.ResetColor();
        }

        public static void BotSpeak(string message)
        {
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.Write("\n  🤖 Bot: ");
            Console.ResetColor();
            TypeEffect(message, ConsoleColor.White);
        }

        public static string UserInput(string prompt = "  👤 You: ")
        {
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.Write("\n" + prompt);
            Console.ResetColor();
            Console.ForegroundColor = ConsoleColor.White;
            string input = Console.ReadLine();
            Console.ResetColor();
            return input ?? "";
        }

        public static void TypeEffect(string text, ConsoleColor color, int delay = 30)
        {
            Console.ForegroundColor = color;
            foreach (char c in text)
            {
                Console.Write(c);
                Thread.Sleep(delay);
            }
            Console.WriteLine();
            Console.ResetColor();
        }

        public static void ShowError(string message)
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine($"\n  ⚠️  {message}");
            Console.ResetColor();
        }
    }

    // ==================== RESPONSE HANDLER ====================
    public class ResponseHandler
    {
        private readonly Dictionary<string, string> _responses;

        public ResponseHandler()
        {
            _responses = new Dictionary<string, string>(StringComparer.OrdinalIgnoreCase)
            {
                // General
                { "how are you",
                  "I'm running securely and efficiently, thank you! Ready to help you stay safe online. 😊" },

                { "what's your purpose",
                  "My purpose is to educate you about cybersecurity! I can help you with:\n" +
                  "  • Recognising phishing emails\n" +
                  "  • Creating strong passwords\n" +
                  "  • Identifying suspicious links\n" +
                  "  • Safe browsing habits" },

                { "what can i ask you",
                  "You can ask me about:\n" +
                  "  • Phishing\n  • Passwords\n  • Safe browsing\n" +
                  "  • Suspicious links\n  • Social engineering\n  • General cybersecurity tips" },

                // Phishing
                { "phishing",
                  "🎣 Phishing is when cybercriminals send fake emails/messages pretending to be legitimate organisations.\n\n" +
                  "  How to spot phishing:\n" +
                  "  ✅ Check the sender's email address carefully\n" +
                  "  ✅ Don't click unexpected links — hover first\n" +
                  "  ✅ Look for urgency tactics like 'Act Now!'\n" +
                  "  ✅ Verify requests via official channels\n" +
                  "  ✅ Poor spelling/grammar is a red flag" },

                // Passwords
                { "password",
                  "🔑 Strong Password Tips:\n" +
                  "  ✅ Use at least 12 characters\n" +
                  "  ✅ Mix uppercase, lowercase, numbers & symbols\n" +
                  "  ✅ Never reuse passwords across sites\n" +
                  "  ✅ Use a password manager\n" +
                  "  ✅ Enable Two-Factor Authentication (2FA)\n" +
                  "  ❌ Never use personal info like birthdays" },

                // Suspicious links
                { "suspicious link",
                  "🔗 How to handle suspicious links:\n" +
                  "  ✅ Hover over the link to see the real URL\n" +
                  "  ✅ Check for HTTPS (padlock icon)\n" +
                  "  ✅ Use a link checker like VirusTotal.com\n" +
                  "  ✅ Avoid shortened URLs from unknown sources\n" +
                  "  ❌ Never click links in unexpected emails/SMS" },

                // Safe browsing
                { "safe browsing",
                  "🌐 Safe Browsing Tips:\n" +
                  "  ✅ Keep your browser updated\n" +
                  "  ✅ Use a reputable antivirus\n" +
                  "  ✅ Only visit HTTPS websites\n" +
                  "  ✅ Avoid public Wi-Fi for sensitive tasks\n" +
                  "  ✅ Clear cookies and cache regularly\n" +
                  "  ✅ Use a VPN on public networks" },

                // Social engineering
                { "social engineering",
                  "🧠 Social Engineering is manipulating people into revealing confidential info.\n\n" +
                  "  Common tactics:\n" +
                  "  • Pretexting (fake scenarios)\n" +
                  "  • Baiting (free offers with malware)\n" +
                  "  • Tailgating (following into secure areas)\n" +
                  "  • Vishing (voice phishing calls)\n\n" +
                  "  Always verify identities before sharing information!" },

                // Malware
                { "malware",
                  "🦠 Malware includes viruses, ransomware, spyware & trojans.\n\n" +
                  "  Protection tips:\n" +
                  "  ✅ Install reputable antivirus software\n" +
                  "  ✅ Keep your OS and apps updated\n" +
                  "  ✅ Don't open unknown email attachments\n" +
                  "  ✅ Back up your data regularly\n" +
                  "  ✅ Avoid downloading software from untrusted sites" },

                // 2FA
                { "two factor",
                  "🔐 Two-Factor Authentication (2FA) adds an extra security layer.\n" +
                  "  Even if your password is stolen, attackers can't access your account.\n\n" +
                  "  Enable 2FA on:\n  • Email accounts\n  • Banking apps\n  • Social media\n  • Any sensitive service" },

                // Goodbye
                { "bye",    "👋 Stay safe online! Goodbye!" },
                { "exit",   "👋 Stay safe online! Goodbye!" },
                { "quit",   "👋 Stay safe online! Goodbye!" }
            };
        }

        public string GetResponse(string input)
        {
            if (string.IsNullOrWhiteSpace(input))
                return null;

            string lowerInput = input.ToLower().Trim();

            foreach (var key in _responses.Keys)
            {
                if (lowerInput.Contains(key))
                    return _responses[key];
            }

            return "";
        }

        public bool IsExitCommand(string input)
        {
            string lower = input.ToLower().Trim();
            return lower == "bye" || lower == "exit" || lower == "quit";
        }
    }

    // ==================== CHATBOT ====================
    public class ChatBot
    {
        private string _userName;
        private readonly ResponseHandler _responseHandler;

        public ChatBot()
        {
            _responseHandler = new ResponseHandler();
        }

        public void Start()
        {
            GreetUser();
            RunConversationLoop();
        }

        private void GreetUser()
        {
            ConsoleUI.BotSpeak("Hello! I'm your Cybersecurity Awareness Assistant.");
            ConsoleUI.BotSpeak("Before we begin, may I know your name?");

            string name = "";
            while (string.IsNullOrWhiteSpace(name))
            {
                name = ConsoleUI.UserInput();
                if (string.IsNullOrWhiteSpace(name))
                {
                    ConsoleUI.ShowError("Invalid input. Please enter your name.");
                }
            }

            _userName = name.Trim();
            ConsoleUI.PrintDivider();
            ConsoleUI.BotSpeak($"Welcome, {_userName}! 🛡️ I'm here to help you stay safe online.");
            ConsoleUI.BotSpeak("You can ask me about: phishing, passwords, suspicious links,");
            ConsoleUI.BotSpeak("safe browsing, social engineering, malware, or type 'bye' to exit.");
            ConsoleUI.PrintDivider();
        }

        private void RunConversationLoop()
        {
            while (true)
            {
                string input = ConsoleUI.UserInput($"  👤 {_userName}: ");

                // Validate input
                if (string.IsNullOrWhiteSpace(input))
                {
                    ConsoleUI.ShowError("It looks like you didn't type anything. Could you rephrase?");
                    continue;
                }

                // Check for exit
                if (_responseHandler.IsExitCommand(input))
                {
                    ConsoleUI.PrintDivider();
                    ConsoleUI.BotSpeak($"Thank you for chatting, {_userName}! Stay safe out there. 🔐");
                    ConsoleUI.PrintDivider();
                    break;
                }

                // Get response
                string response = _responseHandler.GetResponse(input);

                if (response == null)
                {
                    ConsoleUI.ShowError("Invalid input detected. Please type a question.");
                }
                else if (response == "")
                {
                    ConsoleUI.BotSpeak($"I didn't quite understand that, {_userName}. Could you rephrase?");
                    ConsoleUI.BotSpeak("Try asking about: phishing, passwords, safe browsing, or suspicious links.");
                }
                else
                {
                    ConsoleUI.BotSpeak(response);
                }

                ConsoleUI.PrintDivider();
            }
        }
    }
}
