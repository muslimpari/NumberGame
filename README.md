import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.Random;

public class NumberGuessGame extends JFrame implements ActionListener {
    private JTextField guessField;
    private JButton guessButton, restartButton;
    private JLabel messageLabel;
    private int targetNumber;
    private int attempts;

    public NumberGuessGame() {
        // JFrame settings
        setTitle("Number Guessing Game");
        setSize(400, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null); // center the frame
        setLayout(new GridLayout(4, 1));

        // Initialize game
        generateRandomNumber();

        // Components
        JLabel instructions = new JLabel("Guess a number between 1 and 100:", JLabel.CENTER);
        guessField = new JTextField();
        guessButton = new JButton("Guess");
        restartButton = new JButton("Restart Game");
        messageLabel = new JLabel("Enter your guess above.", JLabel.CENTER);

        // Add action listeners
        guessButton.addActionListener(this);
        restartButton.addActionListener(this);

        // Add components to JFrame
        add(instructions);
        add(guessField);
        add(guessButton);
        add(messageLabel);
        add(restartButton);

        // Initially disable restart button
        restartButton.setVisible(false);

        // Make visible
        setVisible(true);
    }

    public void generateRandomNumber() {
        Random rand = new Random();
        targetNumber = rand.nextInt(100) + 1;
        attempts = 0;
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == guessButton) {
            String input = guessField.getText();
            try {
                int guess = Integer.parseInt(input);
                attempts++;

                if (guess == targetNumber) {
                    messageLabel.setText("Correct! You guessed it in " + attempts + " attempts.");
                    guessButton.setEnabled(false);
                    restartButton.setVisible(true);
                } else if (guess < targetNumber) {
                    messageLabel.setText("Too low! Try again.");
                } else {
                    messageLabel.setText("Too high! Try again.");
                }
            } catch (NumberFormatException ex) {
                messageLabel.setText("Please enter a valid number.");
            }
        } else if (e.getSource() == restartButton) {
            generateRandomNumber();
            guessField.setText("");
            messageLabel.setText("Enter your guess above.");
            guessButton.setEnabled(true);
            restartButton.setVisible(false);
        }
    }

    public static void main(String[] args) {
        new NumberGuessGame();
    }
}
