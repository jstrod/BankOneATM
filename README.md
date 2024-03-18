# BankOneATM

First ReadMeFile:

A numbered list of instructions:

1: Start up the GUI in the IDE you're using.
2: Enter your "card" by selecting the button.
3: Enter your PIN to access your bank account.
4: Choose from the main menu what to proceed with.
5: Press the "View Balance" button.
6: View your account details.
7: Press the "Deposit" button.
8: Decide on Cash or Check and submit once other fields are filled.
9: Press the "Withdraw" button.
9: Decide from which account and how much and submit once fields are filled.
10: Press the "Transaction History" button.
11: View Transaction History and Exit once completed.
12: Press the "Exit" button at any time to log out of your account.

Bank One is software used for personal finances. It will house one's bank information and provide them with many options to manage their accounts.

Code:

ATMSystem.java file

package atmsystem;
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;

public class ATMSystem extends JFrame implements ATMSystemInfo {
    private ArrayList<String> users = new ArrayList<>();
    private ArrayList<Integer> pins = new ArrayList<>();
    private ArrayList<Integer> deposits = new ArrayList<>();
    private ArrayList<String> fullnames = new ArrayList<>();

    private JTextField fullNameField;
    private JTextField userNameField;
    private JTextField pinField;
    private JTextField depositField;
    private JTextField withdrawField;
    private JTextArea outputArea;

    private JPanel mainPanel;
    private JPanel registerPanel;
    private JPanel transactionPanel;

    private JButton withdrawButton;
    private JButton viewBalanceButton;
    private JButton depositButton;

    @Override
    public void startATM() {}

    public ATMSystem() {
        setTitle("ATM System");
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setSize(400, 300);
        setLocationRelativeTo(null);

        mainPanel = new JPanel(new CardLayout());

        registerPanel = createRegisterPanel();
        mainPanel.add(registerPanel, "registerPanel");

        add(mainPanel);

        setVisible(true);
    }

    private JPanel createRegisterPanel() {
        JPanel panel = new JPanel(new BorderLayout());

        JPanel inputPanel = new JPanel();
        GroupLayout layout = new GroupLayout(inputPanel);
        inputPanel.setLayout(layout);
        layout.setAutoCreateGaps(true);
        layout.setAutoCreateContainerGaps(true);

        JLabel fullNameLabel = new JLabel("Full Name:");
        JLabel userNameLabel = new JLabel("Username:");
        JLabel pinLabel = new JLabel("PIN:");

        fullNameField = new JTextField();
        userNameField = new JTextField();
        pinField = new JTextField();

        JButton registerButton = new JButton("Register");
        registerButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                registerUser();
            }
        });

        layout.setHorizontalGroup(layout.createSequentialGroup()
                .addGroup(layout.createParallelGroup(GroupLayout.Alignment.LEADING)
                        .addComponent(fullNameLabel)
                        .addComponent(userNameLabel)
                        .addComponent(pinLabel))
                .addGroup(layout.createParallelGroup(GroupLayout.Alignment.LEADING)
                        .addComponent(fullNameField)
                        .addComponent(userNameField)
                        .addComponent(pinField)
                        .addComponent(registerButton)));

        layout.setVerticalGroup(layout.createSequentialGroup()
                .addGroup(layout.createParallelGroup(GroupLayout.Alignment.BASELINE)
                        .addComponent(fullNameLabel)
                        .addComponent(fullNameField))
                .addGroup(layout.createParallelGroup(GroupLayout.Alignment.BASELINE)
                        .addComponent(userNameLabel)
                        .addComponent(userNameField))
                .addGroup(layout.createParallelGroup(GroupLayout.Alignment.BASELINE)
                        .addComponent(pinLabel)
                        .addComponent(pinField))
                .addComponent(registerButton));

        panel.add(inputPanel, BorderLayout.CENTER);

        return panel;
    }

    private void createTransactionPanel() {
        transactionPanel = new JPanel(new BorderLayout());

        JPanel inputPanel = new JPanel();
        GroupLayout layout = new GroupLayout(inputPanel);
        inputPanel.setLayout(layout);
        layout.setAutoCreateGaps(true);
        layout.setAutoCreateContainerGaps(true);

        JLabel depositLabel = new JLabel("Deposit Amount:");
        JLabel withdrawLabel = new JLabel("Withdraw Amount:");

        depositField = new JTextField();
        withdrawField = new JTextField();

        depositButton = new JButton("Deposit");
        depositButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                deposit();
            }
        });

        withdrawButton = new JButton("Withdraw");
        withdrawButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                withdraw();
            }
        });

        viewBalanceButton = new JButton("View Balance");
        viewBalanceButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                viewBalance();
            }
        });

        layout.setHorizontalGroup(layout.createSequentialGroup()
                .addGroup(layout.createParallelGroup(GroupLayout.Alignment.LEADING)
                        .addComponent(depositLabel)
                        .addComponent(withdrawLabel))
                .addGroup(layout.createParallelGroup(GroupLayout.Alignment.LEADING)
                        .addComponent(depositField)
                        .addComponent(withdrawField))
                .addGroup(layout.createParallelGroup(GroupLayout.Alignment.LEADING)
                        .addComponent(depositButton)
                        .addComponent(withdrawButton)
                        .addComponent(viewBalanceButton)));

        layout.setVerticalGroup(layout.createSequentialGroup()
                .addGroup(layout.createParallelGroup(GroupLayout.Alignment.BASELINE)
                        .addComponent(depositLabel)
                        .addComponent(depositField)
                        .addComponent(depositButton))
                .addGroup(layout.createParallelGroup(GroupLayout.Alignment.BASELINE)
                        .addComponent(withdrawLabel)
                        .addComponent(withdrawField)
                        .addComponent(withdrawButton))
                .addComponent(viewBalanceButton));

        transactionPanel.add(inputPanel, BorderLayout.CENTER);
    }

    private void registerUser() {
        String fullName = fullNameField.getText();
        String userName = userNameField.getText();
        String pinText = pinField.getText();

        // Check if any of the fields are empty
        if (fullName.isEmpty() || userName.isEmpty() || pinText.isEmpty()) {
            JOptionPane.showMessageDialog(this, "Please fill in all fields.");
            return;
        }

        try {
            int pin = Integer.parseInt(pinText);

            // Check if the PIN is a 4-digit number
            if (pin < 1000 || pin > 9999) {
                JOptionPane.showMessageDialog(this, "PIN must be a 4-digit number.");
                return;
            }
            if (users.contains(userName)) {
                JOptionPane.showMessageDialog(this, "Username already registered.");
                return;
            }

            fullnames.add(fullName);
            users.add(userName);
            pins.add(pin);

            // Add a default initial deposit value of 0 for the new user
            deposits.add(0);

            // Create the transaction panel after successful registration
            createTransactionPanel();
            mainPanel.add(transactionPanel, "transactionPanel");

            CardLayout cardLayout = (CardLayout) mainPanel.getLayout();
            cardLayout.show(mainPanel, "transactionPanel");
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, "Invalid PIN, please enter only numbers.");
        }
    }

    private void deposit() {
        String userName = userNameField.getText();
        String depositAmountText = depositField.getText();

        int depositAmount;
        try {
            depositAmount = Integer.parseInt(depositAmountText);
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, "Invalid deposit amount.");
            return;
        }

        int index = users.indexOf(userName);
        if (index == -1) {
            JOptionPane.showMessageDialog(this, "User not found.");
            return;
        }

        int currentBalance = deposits.get(index);
        currentBalance += depositAmount;
        deposits.set(index, currentBalance);

        // Display the updated balance
        JOptionPane.showMessageDialog(this, "Deposit successful. Your current balance is: " + currentBalance);
    }

    private void withdraw() {
        String userName = userNameField.getText();
        int withdrawAmount;
        try {
            withdrawAmount = Integer.parseInt(withdrawField.getText());
        } catch (NumberFormatException e) {
            JOptionPane.showMessageDialog(this, "Invalid withdrawal amount.");
            return;
        }

        int index = users.indexOf(userName);
        if (index == -1) {
            JOptionPane.showMessageDialog(this, "User not found.");
            return;
        }

        int currentBalance = deposits.get(index);
        if (withdrawAmount > currentBalance) {
            JOptionPane.showMessageDialog(this, "Insufficient funds.");
        } else {
            currentBalance -= withdrawAmount;
            deposits.set(index, currentBalance);

            // Display the updated balance
            JOptionPane.showMessageDialog(this, "Withdrawal successful. Your current balance is: " + currentBalance);
        }
    }

    private void viewBalance() {
        String userName = userNameField.getText();
        int index = users.indexOf(userName);
        if (index == -1) {
            JOptionPane.showMessageDialog(this, "User not found.");
            return;
        }

        int currentBalance = deposits.get(index);

        // Display the current balance
        JOptionPane.showMessageDialog(this, "Your current balance is: " + currentBalance);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                new ATMSystem();
            }
        });
    }
}

ATMSystemInfo.java File

package atmsystem;
public interface ATMSystemInfo
{
    public void startATM();
}

Explanation for ATMSystem:

The ATMSystem Java file represents our modernized and efficient ATM System. It uses a Swing for the GUI and implements an interface called ATMSystemInfo. Bank One's ATM System allows our users to log in/register, deposit, withdraw, transfer, and view their account balance.

This file is also a part of the atmsystem package. It importans necessary classes from javax.swing.*, java.awt.*, java.awt.event.*, and java.util.*. The ATMSystem class extends a JFrame. A JFrame is a swing component for creating a window and implementing ATMSystemInfo.

ATMSystem class has several instance variables, including ArrayLists for users, pins, deposits, and full names. It also has JTextfield objects for user input and a JTextArea for outputs of user input. There are JPanel objects for different sections of the GUI and JButton objects for user interactions.

The ATMSystem() constructor sets up our GUI. It set the title, default close operation, size, and location of the window. It also initializes the mainPanel and registerPanel that ass the registerPanel to the mainPanel. The startATM() method is an override from the ATMSystenIfno interface. The createRegisterPanel() method creates the registration panel for our GUI.

Explanation for ATMSystemInfo:

This file is an ATMSystemInfo interface that is enlisted in the atmsystem package. This is a great way to group related classes and interfaces. ATMSystemInfo declares a single method of startATM(). An interface is a Java reference type, similar to a class but not quite, but also may contain only constants, method signatures, default methods, static methods, and nested types. It assures that a class adheres to a certain contract. The startATM() method is a method declared in the interface.
