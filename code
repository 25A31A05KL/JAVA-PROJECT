import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.io.*;
import java.nio.file.*;
import java.time.LocalDate;
import java.util.*;
import java.util.List;

public class RythuMitra extends JFrame {

    private static final Color GREEN = new Color(27, 105, 65);
    private static final Color LIGHT = new Color(239, 247, 240);
    private static final String DATA_DIR =
            System.getProperty("user.home") + File.separator + "RythuMitra";
    private static final String DATA_FILE =
            DATA_DIR + File.separator + "data.ser";

    static class Crop implements Serializable {
        String name, area, sowingDate, harvestDate;
        double expectedYield;

        Crop(String n, String a, String s, String h, double y) {
            name=n; area=a; sowingDate=s; harvestDate=h;
            expectedYield=y;
        }
    }

    static class Expense implements Serializable {
        String crop, category, date, note;
        double amount;

        Expense(String c, String cat, double a, String d, String n) {
            crop=c; category=cat; amount=a; date=d; note=n;
        }
    }

    static class MarketPrice implements Serializable {
        String crop, market, date;
        double price;

        MarketPrice(String c, String m, double p, String d) {
            crop=c; market=m; price=p; date=d;
        }
    }

    static class Reminder implements Serializable {
        String title, date;
        boolean completed;

        Reminder(String t, String d) {
            title=t; date=d; completed=false;
        }
    }

    static class AppData implements Serializable {
        ArrayList<Crop> crops = new ArrayList<>();
        ArrayList<Expense> expenses = new ArrayList<>();
        ArrayList<MarketPrice> prices = new ArrayList<>();
        ArrayList<Reminder> reminders = new ArrayList<>();
    }

    private AppData data = new AppData();

    private JPanel dashboard;
    private JTable cropTable, expenseTable, priceTable, reminderTable;
    private JLabel cropCount, expenseTotal, reminderCount, farmerMessage;

    public RythuMitra() {
        loadData();

        setTitle("RythuMitra - Smart Farmer Assistant");
        setSize(1050, 700);
        setMinimumSize(new Dimension(850, 550));
        setDefaultCloseOperation(JFrame.DO_NOTHING_ON_CLOSE);
        setLocationRelativeTo(null);

        addWindowListener(new java.awt.event.WindowAdapter() {
            public void windowClosing(java.awt.event.WindowEvent e) {
                saveData();
                dispose();
                System.exit(0);
            }
        });

        createInterface();
        refreshAll();
    }

    private void createInterface() {
        setLayout(new BorderLayout());

        JPanel header = new JPanel(new BorderLayout());
        header.setBackground(GREEN);
        header.setBorder(BorderFactory.createEmptyBorder(18, 25, 18, 25));

        JLabel title = new JLabel("🌾 RythuMitra");
        title.setForeground(Color.WHITE);
        title.setFont(new Font("Arial", Font.BOLD, 27));

        JLabel subtitle = new JLabel("Smart Farmer Assistant");
        subtitle.setForeground(Color.WHITE);
        subtitle.setFont(new Font("Arial", Font.PLAIN, 14));

        JPanel heading = new JPanel(new GridLayout(2,1));
        heading.setOpaque(false);
        heading.add(title);
        heading.add(subtitle);
        header.add(heading, BorderLayout.WEST);

        JLabel date = new JLabel(LocalDate.now().toString());
        date.setForeground(Color.WHITE);
        header.add(date, BorderLayout.EAST);

        add(header, BorderLayout.NORTH);

        JTabbedPane tabs = new JTabbedPane();
        tabs.setFont(new Font("Arial", Font.BOLD, 14));

        dashboard = new JPanel();
        tabs.addTab("Dashboard", dashboard);

        tabs.addTab("Crop Management", createCropPanel());
        tabs.addTab("Expenses", createExpensePanel());
        tabs.addTab("Market Prices", createPricePanel());
        tabs.addTab("Reminders", createReminderPanel());
        tabs.addTab("Reports", createReportPanel());

        add(tabs, BorderLayout.CENTER);

        JPanel footer = new JPanel();
        footer.setBackground(GREEN);
        JLabel footerText = new JLabel(
                "RythuMitra | Supporting Smart and Sustainable Farming");
        footerText.setForeground(Color.WHITE);
        footer.add(footerText);
        add(footer, BorderLayout.SOUTH);
    }

    private JPanel basePanel() {
        JPanel p = new JPanel(new BorderLayout(10,10));
        p.setBackground(LIGHT);
        p.setBorder(BorderFactory.createEmptyBorder(15,15,15,15));
        return p;
    }

    private JButton button(String text) {
        JButton b = new JButton(text);
        b.setBackground(GREEN);
        b.setForeground(Color.WHITE);
        b.setFocusPainted(false);
        b.setFont(new Font("Arial", Font.BOLD, 13));
        return b;
    }

    private JTable createTable(String... columns) {
        DefaultTableModel model = new DefaultTableModel(columns, 0) {
            public boolean isCellEditable(int r, int c) {
                return false;
            }
        };
        JTable table = new JTable(model);
        table.setRowHeight(27);
        table.setFont(new Font("Arial", Font.PLAIN, 13));
        table.getTableHeader().setFont(new Font("Arial", Font.BOLD, 13));
        return table;
    }

    private DefaultTableModel model(JTable table) {
        return (DefaultTableModel) table.getModel();
    }

    private JPanel createCropPanel() {
        JPanel p = basePanel();
        cropTable = createTable("Crop", "Land (Acres)",
                "Sowing Date", "Harvest Date", "Expected Yield (Quintals)");

        JButton add = button("Add Crop");
        add.addActionListener(e -> addCrop());

        JButton delete = button("Delete Selected");
        delete.addActionListener(e -> {
            int row = cropTable.getSelectedRow();
            if(row >= 0) {
                data.crops.remove(row);
                refreshAll();
                saveData();
            } else showMessage("Select a crop first.");
        });

        JPanel actions = new JPanel(new FlowLayout(FlowLayout.LEFT));
        actions.setOpaque(false);
        actions.add(add);
        actions.add(delete);

        p.add(actions, BorderLayout.NORTH);
        p.add(new JScrollPane(cropTable), BorderLayout.CENTER);
        return p;
    }

    private JPanel createExpensePanel() {
        JPanel p = basePanel();
        expenseTable = createTable("Crop", "Category", "Amount (Rs.)",
                "Date", "Description");

        JButton add = button("Add Expense");
        add.addActionListener(e -> addExpense());

        JButton delete = button("Delete Selected");
        delete.addActionListener(e -> {
            int row = expenseTable.getSelectedRow();
            if(row >= 0) {
                data.expenses.remove(row);
                refreshAll();
                saveData();
            } else showMessage("Select an expense first.");
        });

        JPanel actions = new JPanel(new FlowLayout(FlowLayout.LEFT));
        actions.setOpaque(false);
        actions.add(add);
        actions.add(delete);

        p.add(actions, BorderLayout.NORTH);
        p.add(new JScrollPane(expenseTable), BorderLayout.CENTER);
        return p;
    }

    private JPanel createPricePanel() {
        JPanel p = basePanel();
        priceTable = createTable("Crop", "Market", "Price / Quintal (Rs.)",
                "Date");

        JButton add = button("Add Market Price");
        add.addActionListener(e -> addPrice());

        JButton delete = button("Delete Selected");
        delete.addActionListener(e -> {
            int row = priceTable.getSelectedRow();
            if(row >= 0) {
                data.prices.remove(row);
                refreshAll();
                saveData();
            } else showMessage("Select a price first.");
        });

        JPanel actions = new JPanel(new FlowLayout(FlowLayout.LEFT));
        actions.setOpaque(false);
        actions.add(add);
        actions.add(delete);

        p.add(actions, BorderLayout.NORTH);
        p.add(new JScrollPane(priceTable), BorderLayout.CENTER);
        return p;
    }

    private JPanel createReminderPanel() {
        JPanel p = basePanel();
        reminderTable = createTable("Task", "Due Date", "Status");

        JButton add = button("Add Reminder");
        add.addActionListener(e -> addReminder());

        JButton done = button("Mark Completed");
        done.addActionListener(e -> {
            int row = reminderTable.getSelectedRow();
            if(row >= 0) {
                data.reminders.get(row).completed = true;
                refreshAll();
                saveData();
            } else showMessage("Select a reminder first.");
        });

        JButton delete = button("Delete Selected");
        delete.addActionListener(e -> {
            int row = reminderTable.getSelectedRow();
            if(row >= 0) {
                data.reminders.remove(row);
                refreshAll();
                saveData();
            } else showMessage("Select a reminder first.");
        });

        JPanel actions = new JPanel(new FlowLayout(FlowLayout.LEFT));
        actions.setOpaque(false);
        actions.add(add);
        actions.add(done);
        actions.add(delete);

        p.add(actions, BorderLayout.NORTH);
        p.add(new JScrollPane(reminderTable), BorderLayout.CENTER);
        return p;
    }

    private JPanel createReportPanel() {
        JPanel p = basePanel();
        p.setLayout(new BoxLayout(p, BoxLayout.Y_AXIS));

        JLabel heading = new JLabel("Farm Financial Report");
        heading.setFont(new Font("Arial", Font.BOLD, 23));

        JTextArea report = new JTextArea();
        report.setEditable(false);
        report.setFont(new Font("Monospaced", Font.PLAIN, 14));
        report.setBackground(Color.WHITE);

        JButton generate = button("Generate Report");
        generate.addActionListener(e -> {
            report.setText(buildReport());
        });

        p.add(heading);
        p.add(Box.createVerticalStrut(15));
        p.add(generate);
        p.add(Box.createVerticalStrut(15));
        p.add(new JScrollPane(report));

        return p;
    }

    private void addCrop() {
        JTextField name = new JTextField();
        JTextField area = new JTextField();
        JTextField sow = new JTextField(LocalDate.now().toString());
        JTextField harvest = new JTextField();
        JTextField yield = new JTextField();

        JPanel form = formPanel(
                "Crop Name", name,
                "Land Area (Acres)", area,
                "Sowing Date (YYYY-MM-DD)", sow,
                "Expected Harvest Date", harvest,
                "Expected Yield (Quintals)", yield
        );

        if(JOptionPane.showConfirmDialog(this, form,
                "Add New Crop", JOptionPane.OK_CANCEL_OPTION)
                == JOptionPane.OK_OPTION) {
            try {
                if(name.getText().trim().isEmpty())
                    throw new IllegalArgumentException("Crop name is required.");

                double a = Double.parseDouble(area.getText().trim());
                double y = Double.parseDouble(yield.getText().trim());

                if(a <= 0 || y < 0)
                    throw new IllegalArgumentException("Enter valid area and yield.");

                LocalDate.parse(sow.getText().trim());
                LocalDate.parse(harvest.getText().trim());

                data.crops.add(new Crop(name.getText().trim(),
                        area.getText().trim(), sow.getText().trim(),
                        harvest.getText().trim(), y));

                refreshAll();
                saveData();
            } catch(Exception ex) {
                showMessage("Invalid crop details. Check numbers and dates.");
            }
        }
    }

    private void addExpense() {
        JTextField crop = new JTextField();
        JComboBox<String> category = new JComboBox<>(new String[]{
                "Seeds", "Fertilizer", "Labour", "Irrigation",
                "Transport", "Machinery", "Pesticides", "Other"
        });
        JTextField amount = new JTextField();
        JTextField date = new JTextField(LocalDate.now().toString());
        JTextField note = new JTextField();

        JPanel form = formPanel(
                "Crop Name", crop,
                "Category", category,
                "Amount (Rs.)", amount,
                "Date (YYYY-MM-DD)", date,
                "Description", note
        );

        if(JOptionPane.showConfirmDialog(this, form,
                "Add Farming Expense", JOptionPane.OK_CANCEL_OPTION)
                == JOptionPane.OK_OPTION) {
            try {
                double a = Double.parseDouble(amount.getText().trim());
                if(a <= 0) throw new IllegalArgumentException();
                LocalDate.parse(date.getText().trim());

                data.expenses.add(new Expense(crop.getText().trim(),
                        category.getSelectedItem().toString(),
                        a, date.getText().trim(), note.getText().trim()));

                refreshAll();
                saveData();
            } catch(Exception ex) {
                showMessage("Enter a valid amount and date.");
            }
        }
    }

    private void addPrice() {
        JTextField crop = new JTextField();
        JTextField market = new JTextField();
        JTextField price = new JTextField();
        JTextField date = new JTextField(LocalDate.now().toString());

        JPanel form = formPanel(
                "Crop Name", crop,
                "Market Name", market,
                "Price per Quintal (Rs.)", price,
                "Date (YYYY-MM-DD)", date
        );

        if(JOptionPane.showConfirmDialog(this, form,
                "Add Market Price", JOptionPane.OK_CANCEL_OPTION)
                == JOptionPane.OK_OPTION) {
            try {
                double value = Double.parseDouble(price.getText().trim());
                if(value <= 0) throw new IllegalArgumentException();
                LocalDate.parse(date.getText().trim());

                data.prices.add(new MarketPrice(
                        crop.getText().trim(), market.getText().trim(),
                        value, date.getText().trim()));

                refreshAll();
                saveData();
            } catch(Exception ex) {
                showMessage("Enter a valid price and date.");
            }
        }
    }

    private void addReminder() {
        JTextField title = new JTextField();
        JTextField date = new JTextField(LocalDate.now().toString());

        JPanel form = formPanel(
                "Task Description", title,
                "Due Date (YYYY-MM-DD)", date
        );

        if(JOptionPane.showConfirmDialog(this, form,
                "Add Farming Reminder", JOptionPane.OK_CANCEL_OPTION)
                == JOptionPane.OK_OPTION) {
            try {
                if(title.getText().trim().isEmpty())
                    throw new IllegalArgumentException();

                LocalDate.parse(date.getText().trim());
                data.reminders.add(new Reminder(
                        title.getText().trim(), date.getText().trim()));

                refreshAll();
                saveData();
            } catch(Exception ex) {
                showMessage("Enter a task and valid date.");
            }
        }
    }

    private JPanel formPanel(Object... fields) {
        JPanel panel = new JPanel(new GridLayout(0,2,8,10));
        panel.setBorder(BorderFactory.createEmptyBorder(10,10,10,10));

        for(int i=0; i<fields.length; i+=2) {
            panel.add(new JLabel(fields[i].toString()));
            panel.add((Component)fields[i+1]);
        }
        return panel;
    }

    private void createDashboard() {
        dashboard.removeAll();
        dashboard.setBackground(LIGHT);
        dashboard.setLayout(new BorderLayout(15,15));
        dashboard.setBorder(BorderFactory.createEmptyBorder(25,25,25,25));

        JLabel welcome = new JLabel("Welcome to RythuMitra 🌾");
        welcome.setFont(new Font("Arial", Font.BOLD, 26));

        JPanel cards = new JPanel(new GridLayout(1,3,15,15));
        cards.setOpaque(false);

        cropCount = new JLabel();
        expenseTotal = new JLabel();
        reminderCount = new JLabel();

        cards.add(statCard("Total Crops", cropCount));
        cards.add(statCard("Total Expenses", expenseTotal));
        cards.add(statCard("Pending Tasks", reminderCount));

        farmerMessage = new JLabel(
                "<html><h2>Manage your farm smartly!</h2>"
                + "Track crops, expenses, prices and important tasks."
                + "</html>");
        farmerMessage.setFont(new Font("Arial", Font.PLAIN, 17));

        dashboard.add(welcome, BorderLayout.NORTH);
        dashboard.add(cards, BorderLayout.CENTER);
        dashboard.add(farmerMessage, BorderLayout.SOUTH);
        dashboard.revalidate();
        dashboard.repaint();
    }

    private JPanel statCard(String heading, JLabel value) {
        JPanel card = new JPanel(new GridLayout(2,1,5,5));
        card.setBackground(Color.WHITE);
        card.setBorder(BorderFactory.createCompoundBorder(
                BorderFactory.createLineBorder(new Color(210,225,210)),
                BorderFactory.createEmptyBorder(20,15,20,15)));

        JLabel title = new JLabel(heading);
        title.setFont(new Font("Arial", Font.BOLD, 16));
        title.setForeground(GREEN);

        value.setFont(new Font("Arial", Font.BOLD, 24));
        value.setForeground(new Color(35,75,45));

        card.add(title);
        card.add(value);
        return card;
    }

    private void refreshAll() {
        if(cropTable != null) {
            model(cropTable).setRowCount(0);
            for(Crop c : data.crops)
                model(cropTable).addRow(new Object[]{
                        c.name,c.area,c.sowingDate,c.harvestDate,c.expectedYield
                });
        }

        if(expenseTable != null) {
            model(expenseTable).setRowCount(0);
            for(Expense e : data.expenses)
                model(expenseTable).addRow(new Object[]{
                        e.crop,e.category,e.amount,e.date,e.note
                });
        }

        if(priceTable != null) {
            model(priceTable).setRowCount(0);
            for(MarketPrice p : data.prices)
                model(priceTable).addRow(new Object[]{
                        p.crop,p.market,p.price,p.date
                });
        }

        if(reminderTable != null) {
            model(reminderTable).setRowCount(0);
            for(Reminder r : data.reminders)
                model(reminderTable).addRow(new Object[]{
                        r.title,r.date,
                        r.completed ? "Completed" :
                        (r.date.compareTo(LocalDate.now().toString()) < 0
                                ? "Overdue" : "Pending")
                });
        }

        if(dashboard != null) {
            createDashboard();
            cropCount.setText(String.valueOf(data.crops.size()));

            double total = 0;
            for(Expense e : data.expenses) total += e.amount;
            expenseTotal.setText(String.format("Rs. %.2f",total));

            long pending = data.reminders.stream()
                    .filter(r -> !r.completed).count();
            reminderCount.setText(String.valueOf(pending));
        }
    }

    private String buildReport() {
        StringBuilder sb = new StringBuilder();
        sb.append("        RYTHUMITRA FARM REPORT\n");
        sb.append("====================================\n");
        sb.append("Date: ").append(LocalDate.now()).append("\n\n");

        double totalExpenses = 0;
        for(Expense e : data.expenses) totalExpenses += e.am
