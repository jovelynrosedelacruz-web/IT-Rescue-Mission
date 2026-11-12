import java.util.*;
import java.io.*;

public class Main {
    static Scanner sc = new Scanner(System.in);
    static final String PROFILE_FILE = "profiles.txt";
    static final String HISTORY_FILE = "game_history.txt";

    static class Question {
        String question;
        String[] choices;
        int answer;

        Question(String q, String[] c, int a) {
            question = q;
            choices = c;
            answer = a;
        }
    }

    public static void main(String[] args) {
        while (true) {
            title();

            System.out.println("1. Register Player");
            System.out.println("2. Choose Player");
            System.out.println("3. Exit");

            int choice = getNumber("Select: ", 1, 3);
            String player = "";

            if (choice == 1) player = register();
            else if (choice == 2) player = choosePlayer();
            else {
                System.out.println("Thank you for playing!");
                break;
            }

            if (!player.isEmpty()) playerMenu(player);
        }
    }

    static void title() {
        System.out.println("\n==================================");
        System.out.println("        IT RESCUE MISSION");
        System.out.println("       VERSION 9 - REWARDS");
        System.out.println("==================================");
    }

    static String register() {
        System.out.print("Enter player name: ");
        String name = sc.nextLine().trim();

        if (name.isEmpty() || name.contains("|")) {
            System.out.println("Invalid name.");
            return "";
        }

        for (String p : loadProfiles()) {
            if (p.equalsIgnoreCase(name)) {
                System.out.println("Player already exists.");
                return "";
            }
        }

        try (FileWriter fw = new FileWriter(PROFILE_FILE, true)) {
            fw.write(name + "\n");
            System.out.println("Registration successful!");
            return name;
        } catch (IOException e) {
            System.out.println("Could not save player.");
            return "";
        }
    }

    static String choosePlayer() {
        ArrayList<String> profiles = loadProfiles();

        if (profiles.isEmpty()) {
            System.out.println("No registered players.");
            return "";
        }

        for (int i = 0; i < profiles.size(); i++) {
            System.out.println((i + 1) + ". " + profiles.get(i));
        }

        int choice = getNumber("Choose player: ", 1, profiles.size());
        return profiles.get(choice - 1);
    }

    static void playerMenu(String player) {
        while (true) {
            System.out.println("\n===== WELCOME, " + player.toUpperCase() + " =====");
            System.out.println("1. Start Mission");
            System.out.println("2. Leaderboard");
            System.out.println("3. My Statistics");
            System.out.println("4. Game History");
            System.out.println("5. My Achievements");
            System.out.println("6. Switch Player");
            System.out.println("7. Exit");

            int choice = getNumber("Select: ", 1, 7);

            if (choice == 1) playGame(player);
            else if (choice == 2) leaderboard();
            else if (choice == 3) statistics(player);
            else if (choice == 4) history(player);
            else if (choice == 5) achievements(player);
            else if (choice == 6) return;
            else System.exit(0);
        }
    }

    static void playGame(String player) {
        System.out.println("\n===== MISSION BRIEFING =====");
        System.out.println("Agent: " + player);
        System.out.println("Lives: 3 | Correct answer: 10 points");
        System.out.println("Perfect mission bonus: 5 points");

        System.out.println("\n1. Easy");
        System.out.println("2. Medium");
        System.out.println("3. Hard");

        int level = getNumber("Difficulty: ", 1, 3);
        ArrayList<Question> questions = getQuestions(level);
        Collections.shuffle(questions);

        int score = 0, lives = 3, correct = 0;

        for (int i = 0; i < 3 && lives > 0; i++) {
            Question q = questions.get(i);

            System.out.println("\nQUESTION " + (i + 1) + "/3");
            System.out.println(q.question);

            for (int j = 0; j < 4; j++) {
                System.out.println((j + 1) + ". " + q.choices[j]);
            }

            System.out.println("Lives: " + lives);
            int answer = getNumber("Your answer: ", 1, 4);

            if (answer == q.answer) {
                score += 10;
                correct++;
                System.out.println("CORRECT! +10 points");
            } else {
                lives--;
                System.out.println("WRONG! Correct answer: "
                    + q.choices[q.answer - 1]);
            }
        }

        if (correct == 3) {
            score += 5;
            System.out.println("PERFECT MISSION BONUS: +5");
        }

        System.out.println("\n===== MISSION REPORT =====");
        System.out.println("Score: " + score);
        System.out.println("Correct answers: " + correct + "/3");
        System.out.println("Remaining lives: " + lives);

        saveScore(player, score);
        showNewRewards(player, score);
    }

    static ArrayList<Question> getQuestions(int level) {
        ArrayList<Question> list = new ArrayList<>();

        if (level == 1) {
            list.add(new Question("What does CPU stand for?",
                new String[]{"Central Processing Unit", "Computer Personal Unit",
                "Central Program Utility", "Control Power Unit"}, 1));
            list.add(new Question("Which device is used to type?",
                new String[]{"Monitor", "Keyboard", "Speaker", "Printer"}, 2));
            list.add(new Question("Which is an input device?",
                new String[]{"Monitor", "Projector", "Mouse", "Speaker"}, 3));
            list.add(new Question("What does RAM provide?",
                new String[]{"Temporary memory", "Printing", "Internet cables",
                "Sound output"}, 1));
            list.add(new Question("Which is a web browser?",
                new String[]{"Windows", "Google Chrome", "Excel", "Paint"}, 2));
        } else if (level == 2) {
            list.add(new Question("What does URL mean?",
                new String[]{"Uniform Resource Locator", "User Reading Link",
                "Universal Record List", "United Resource Line"}, 1));
            list.add(new Question("Which is an operating system?",
                new String[]{"Java", "HTML", "Linux", "Python"}, 3));
            list.add(new Question("What does HTML create?",
                new String[]{"Web page structure", "Hardware",
                "Antivirus", "Database server"}, 1));
            list.add(new Question("Which is a strong password practice?",
                new String[]{"Use your name", "Share your password",
                "Use a unique password", "Use 123456"}, 3));
            list.add(new Question("What does a firewall do?",
                new String[]{"Cool the computer", "Filter network traffic",
                "Edit photos", "Increase screen size"}, 2));
        } else {
            list.add(new Question("Which data structure uses FIFO?",
                new String[]{"Stack", "Queue", "Tree", "Graph"}, 2));
            list.add(new Question("What does SQL manage?",
                new String[]{"Databases", "Animations", "Sound files",
                "Computer screens"}, 1));
            list.add(new Question("Which concept hides internal details?",
                new String[]{"Inheritance", "Abstraction", "Iteration",
                "Compilation"}, 2));
            list.add(new Question("What is an algorithm?",
                new String[]{"Step-by-step procedure", "Computer part",
                "Programming language", "Storage device"}, 1));
            list.add(new Question("Which is a Java loop?",
                new String[]{"repeat-only", "for", "display", "input"}, 2));
        }

        return list;
    }

    static void saveScore(String player, int score) {
        try (FileWriter fw = new FileWriter(HISTORY_FILE, true)) {
            fw.write(player + "|" + score + "\n");
            System.out.println("Game record saved!");
        } catch (IOException e) {
            System.out.println("Could not save game record.");
        }
    }

    static ArrayList<String> loadProfiles() {
        ArrayList<String> list = new ArrayList<>();

        try (BufferedReader br = new BufferedReader(
                new FileReader(PROFILE_FILE))) {
            String line;
            while ((line = br.readLine()) != null) {
                if (!line.trim().isEmpty()) list.add(line.trim());
            }
        } catch (IOException e) {
            // File not created yet.
        }

        return list;
    }

    static ArrayList<String[]> loadHistory() {
        ArrayList<String[]> list = new ArrayList<>();

        try (BufferedReader br = new BufferedReader(
                new FileReader(HISTORY_FILE))) {
            String line;
            while ((line = br.readLine()) != null) {
                String[] parts = line.split("\\|");
                if (parts.length == 2) list.add(parts);
            }
        } catch (IOException e) {
            // File not created yet.
        }

        return list;
    }

    static void showNewRewards(String player, int score) {
        System.out.println("\n===== REWARDS EARNED =====");

        ArrayList<String[]> records = loadHistory();
        int games = 0, total = 0;

        for (String[] r : records) {
            if (r[0].equalsIgnoreCase(player)) {
                games++;
                total += Integer.parseInt(r[1]);
            }
        }

        System.out.println("[BADGE] First Mission: "
            + (games >= 1 ? "UNLOCKED" : "LOCKED"));
        System.out.println("[BADGE] Perfect Agent: "
            + (score == 35 ? "UNLOCKED" : "LOCKED"));
        System.out.println("[BADGE] IT Expert: "
            + (total >= 100 ? "UNLOCKED" : "LOCKED"));
        System.out.println("[STAR] Mission Stars: " + (score == 35 ? 1 : 0));
    }

    static void achievements(String player) {
        ArrayList<String[]> records = loadHistory();
        int games = 0, total = 0;
        boolean perfect = false;

        for (String[] r : records) {
            if (r[0].equalsIgnoreCase(player)) {
                int score = Integer.parseInt(r[1]);
                games++;
                total += score;
                if (score == 35) perfect = true;
            }
        }

        System.out.println("\n===== MY ACHIEVEMENTS =====");
        System.out.println("First Mission Badge: "
            + (games >= 1 ? "UNLOCKED" : "LOCKED"));
        System.out.println("Perfect Agent Badge: "
            + (perfect ? "UNLOCKED" : "LOCKED"));
        System.out.println("IT Expert Badge: "
            + (total >= 100 ? "UNLOCKED" : "LOCKED"));
        System.out.println("Total Mission Stars: " + countStars(player));
    }

    static int countStars(String player) {
        int stars = 0;

        for (String[] r : loadHistory()) {
            if (r[0].equalsIgnoreCase(player)
                    && Integer.parseInt(r[1]) == 35) {
                stars++;
            }
        }

        return stars;
    }

    static void leaderboard() {
        ArrayList<String[]> records = loadHistory();

        records.sort((a, b) ->
            Integer.compare(Integer.parseInt(b[1]),
                            Integer.parseInt(a[1])));

        System.out.println("\n===== TOP 5 LEADERBOARD =====");
        for (int i = 0; i < Math.min(5, records.size()); i++) {
            System.out.println((i + 1) + ". " + records.get(i)[0]
                + " - " + records.get(i)[1] + " points");
        }

        if (records.isEmpty()) System.out.println("No scores yet.");
    }

    static void statistics(String player) {
        int games = 0, total = 0, best = 0;

        for (String[] r : loadHistory()) {
            if (r[0].equalsIgnoreCase(player)) {
                int score = Integer.parseInt(r[1]);
                games++;
                total += score;
                if (score > best) best = score;
            }
        }

        System.out.println("\n===== MY STATISTICS =====");
        System.out.println("Player: " + player);
        System.out.println("Games played: " + games);
        System.out.println("Total points: " + total);
        System.out.println("Best score: " + best);

        if (games > 0) {
            System.out.printf("Average: %.2f%n", (double) total / games);
        }
    }

    static void history(String player) {
        boolean found = false;

        System.out.println("\n===== GAME HISTORY =====");
        for (String[] r : loadHistory()) {
            if (r[0].equalsIgnoreCase(player)) {
                System.out.println("Score: " + r[1]);
                found = true;
            }
        }

        if (!found) System.out.println("No game history.");
    }

    static int getNumber(String prompt, int min, int max) {
        while (true) {
            System.out.print(prompt);
            String input = sc.nextLine();

            try {
                int n = Integer.parseInt(input);
                if (n >= min && n <= max) return n;
            } catch (NumberFormatException e) {
                // Invalid input.
            }

            System.out.println("Enter a number from " + min + " to " + max + ".");
        }
    }
}
