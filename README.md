# SCT_SD_01

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class TemperatureConverterGUI {

    public static void main(String[] args) {
        SwingUtilities.invokeLater(TemperatureConverterGUI::createAndShowGUI);
    }

    public static void createAndShowGUI() {
        JFrame frame = new JFrame("Temperature Converter");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(400, 250);
        frame.setLayout(new GridBagLayout());

        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);

        JLabel inputLabel = new JLabel("Enter Temperature:");
        JTextField inputField = new JTextField(10);
        JLabel fromLabel = new JLabel("From:");
        JLabel toLabel = new JLabel("To:");

        String[] scales = {"Celsius", "Fahrenheit", "Kelvin"};
        JComboBox<String> fromCombo = new JComboBox<>(scales);
        JComboBox<String> toCombo = new JComboBox<>(scales);

        JButton convertButton = new JButton("Convert");
        JLabel resultLabel = new JLabel("Result: ");

        gbc.gridx = 0; gbc.gridy = 0;
        frame.add(inputLabel, gbc);
        gbc.gridx = 1;
        frame.add(inputField, gbc);

        gbc.gridx = 0; gbc.gridy = 1;
        frame.add(fromLabel, gbc);
        gbc.gridx = 1;
        frame.add(fromCombo, gbc);

        gbc.gridx = 0; gbc.gridy = 2;
        frame.add(toLabel, gbc);
        gbc.gridx = 1;
        frame.add(toCombo, gbc);

        gbc.gridx = 0; gbc.gridy = 3;
        frame.add(convertButton, gbc);
        gbc.gridx = 1;
        frame.add(resultLabel, gbc);

        convertButton.addActionListener(e -> {
            try {
                double inputTemp = Double.parseDouble(inputField.getText());
                String from = (String) fromCombo.getSelectedItem();
                String to = (String) toCombo.getSelectedItem();
                double result = convertTemperature(inputTemp, from, to);
                resultLabel.setText(String.format("Result: %.2f %s", result, to));
            } catch (NumberFormatException ex) {
                JOptionPane.showMessageDialog(frame, "Please enter a valid number.", "Input Error", JOptionPane.ERROR_MESSAGE);
            }
        });

        frame.setLocationRelativeTo(null);
        frame.setVisible(true);
    }

    public static double convertTemperature(double temp, String from, String to) {
        // Convert input to Celsius first
        double tempInCelsius = switch (from) {
            case "Fahrenheit" -> (temp - 32) * 5/9;
            case "Kelvin" -> temp - 273.15;
            default -> temp;
        };

        // Convert from Celsius to target
        return switch (to) {
            case "Fahrenheit" -> (tempInCelsius * 9/5) + 32;
            case "Kelvin" -> tempInCelsius + 273.15;
            default -> tempInCelsius;
        };
    }
}
