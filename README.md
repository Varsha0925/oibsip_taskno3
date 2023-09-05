# oibsip_taskno3
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class ATMInterface {
    private static Map<String, Double> accountBalances = new HashMap<>();
    private static Map<String, String> accountPins = new HashMap<>();
    private static Map<String, String> transactionHistory = new HashMap<>();
    private static String currentUser = null;

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
		// Sample account numbers and PINs 
        accountPins.put("123456", "1234"); // Account number "123456" with PIN "1234"
        accountPins.put("789012", "5678"); // Account number "789012" with PIN "5678"

        // Initialize account balances
        accountBalances.put("123456", 1000.0); // Starting balance for account "123456"
        accountBalances.put("789012", 1500.0); // Starting balance for account "789012"


        while (true) {
            if (currentUser == null) {
                System.out.println("\nWelcome to the ATM");
                System.out.println("1. Log in");
                System.out.println("2. Quit");
                System.out.print("Select an option: ");

                int choice = scanner.nextInt();
                scanner.nextLine(); // Consume newline

                switch (choice) {
                    case 1:
                        login(scanner);
                        break;
                    case 2:
                        System.out.println("Thank you for using the ATM. Goodbye!");
                        scanner.close();
                        System.exit(0);
                    default:
                        System.out.println("Invalid option. Please try again.");
                }
            } else {
                System.out.println("\nATM Menu");
                System.out.println("1. Withdraw");
                System.out.println("2. Deposit");
                System.out.println("3. Transfer");
                System.out.println("4. Transaction History");
                System.out.println("5. Logout");
                System.out.print("Select an option: ");

                int choice = scanner.nextInt();
                scanner.nextLine(); // Consume newline

                switch (choice) {
                    case 1:
                        withdraw(scanner);
                        break;
                    case 2:
                        deposit(scanner);
                        break;
                    case 3:
                        transfer(scanner);
                        break;
                    case 4:
                        showTransactionHistory();
                        break;
                    case 5:
                        logout();
                        break;
                    default:
                        System.out.println("Invalid option. Please try again.");
                }
            }
        }
    }

    private static void login(Scanner scanner) {
        System.out.print("Enter your account number: ");
        String accountNumber = scanner.nextLine();
        System.out.print("Enter your PIN: ");
        String pin = scanner.nextLine();

        if (accountPins.containsKey(accountNumber) && accountPins.get(accountNumber).equals(pin)) {
            currentUser = accountNumber;
            System.out.println("Login successful. Welcome, " + accountNumber + "!");
        } else {
            System.out.println("Login failed. Please check your account number and PIN.");
        }
    }

    private static void withdraw(Scanner scanner) {
        System.out.print("Enter the amount to withdraw: $");
        double amount = scanner.nextDouble();

        if (amount > 0 && accountBalances.get(currentUser) >= amount) {
            accountBalances.put(currentUser, accountBalances.get(currentUser) - amount);
            addToTransactionHistory("Withdraw: -$" + amount);
            System.out.println("Withdrawal successful.");
        } else {
            System.out.println("Invalid amount or insufficient funds.");
        }
    }

    private static void deposit(Scanner scanner) {
        System.out.print("Enter the amount to deposit: $");
        double amount = scanner.nextDouble();

        if (amount > 0) {
            accountBalances.put(currentUser, accountBalances.get(currentUser) + amount);
            addToTransactionHistory("Deposit: +$" + amount);
            System.out.println("Deposit successful.");
        } else {
            System.out.println("Invalid amount.");
        }
    }

    private static void transfer(Scanner scanner) {
        System.out.print("Enter the recipient's account number: ");
        String recipient = scanner.nextLine();

        if (!accountBalances.containsKey(recipient) || recipient.equals(currentUser)) {
            System.out.println("Invalid recipient account.");
            return;
        }

        System.out.print("Enter the amount to transfer: $");
        double amount = scanner.nextDouble();

        if (amount > 0 && accountBalances.get(currentUser) >= amount) {
            accountBalances.put(currentUser, accountBalances.get(currentUser) - amount);
            accountBalances.put(recipient, accountBalances.get(recipient) + amount);
            addToTransactionHistory("Transfer to " + recipient + ": -$" + amount);
            System.out.println("Transfer successful.");
        } else {
            System.out.println("Invalid amount or insufficient funds.");
        }
    }

    private static void showTransactionHistory() {
        String history = transactionHistory.get(currentUser);
        if (history != null) {
            System.out.println("\nTransaction History:");
            System.out.println(history);
        } else {
            System.out.println("No transaction history available.");
        }
    }

    private static void addToTransactionHistory(String transaction) {
        String history = transactionHistory.getOrDefault(currentUser, "");
        history += transaction + "\n";
        transactionHistory.put(currentUser, history);
    }

    private static void logout() {
        currentUser = null;
        System.out.println("Logged out successfully.");
    }
}
