# Sample
Artiffai tech's first repository
import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.TextField;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class CalculatorApp extends Application {

    private TextField displayField;
    private double currentNumber = 0;
    private String operator = "";
    private boolean startNewNumber = true;

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("My Calculator");

        // UI Elements
        VBox root = new VBox(10);
        root.setPadding(new Insets(10));
        root.setAlignment(Pos.CENTER);

        // Display Field
        displayField = new TextField("0");
        displayField.setEditable(false);
        displayField.setAlignment(Pos.CENTER_RIGHT);
        displayField.setPrefHeight(40);

        // Number and Operator Buttons
        GridPane buttonPane = new GridPane();
        buttonPane.setHgap(10);
        buttonPane.setVgap(10);
        buttonPane.setAlignment(Pos.CENTER);

        String[][] buttonLabels = {
            {"7", "8", "9", "/"},
            {"4", "5", "6", "*"},
            {"1", "2", "3", "-"},
            {"0", "C", "=", "+"}
        };

        for (int row = 0; row < buttonLabels.length; row++) {
            for (int col = 0; col < buttonLabels[row].length; col++) {
                String label = buttonLabels[row][col];
                Button button = new Button(label);
                button.setPrefSize(50, 50);
                button.setOnAction(e -> handleButtonClick(label));
                buttonPane.add(button, col, row);
            }
        }

        root.getChildren().addAll(displayField, buttonPane);

        Scene scene = new Scene(root, 250, 350);
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    private void handleButtonClick(String buttonLabel) {
        if (buttonLabel.matches("[0-9]")) {
            if (startNewNumber) {
                displayField.setText(buttonLabel);
                startNewNumber = false;
            } else {
                displayField.appendText(buttonLabel);
            }
        } else if (buttonLabel.equals("C")) {
            displayField.setText("0");
            currentNumber = 0;
            operator = "";
            startNewNumber = true;
        } else if (buttonLabel.matches("[+\\-*/]")) {
            try {
                currentNumber = Double.parseDouble(displayField.getText());
            } catch (NumberFormatException e) {
                displayField.setText("Error");
                startNewNumber = true;
                return;
            }
            operator = buttonLabel;
            startNewNumber = true;
        } else if (buttonLabel.equals("=")) {
            if (!operator.isEmpty()) {
                double secondNumber;
                try {
                    secondNumber = Double.parseDouble(displayField.getText());
                } catch (NumberFormatException e) {
                    displayField.setText("Error");
                    startNewNumber = true;
                    return;
                }

                double result = 0;
                try {
                    switch (operator) {
                        case "+":
                            result = currentNumber + secondNumber;
                            break;
                        case "-":
                            result = currentNumber - secondNumber;
                            break;
                        case "*":
                            result = currentNumber * secondNumber;
                            break;
                        case "/":
                            if (secondNumber == 0) {
                                throw new ArithmeticException("Division by zero");
                            }
                            result = currentNumber / secondNumber;
                            break;
                    }
                    displayField.setText(String.valueOf(result));
                } catch (ArithmeticException e) {
                    displayField.setText("Error: " + e.getMessage());
                }
                operator = "";
                startNewNumber = true;
            }
        }
    }

    public static void main(String[] args) {
        launch(args);
    }
}
