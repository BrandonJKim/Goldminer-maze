import java.util.*;

public class Main {
    private static final Scanner scanner = new Scanner(System.in);
    private static final Random rand = new Random();
    private static final Random nums = new Random();
    private static double goldz = 0;
    private static double goldMultiplier = 1;
    private static boolean mazeEntered = false;
    private static Set<String> inventory = new HashSet<>();
    private static int health = 100;
    private static int mazeX = 0;
    private static int mazeY = 0;

    public static void gold() {
        while (true) {
            System.out.printf("You have %.0f gold.\n", goldz);

            if (goldz >= 1000 && !mazeEntered) {
                System.out.println("\nYou’ve reached 1000 gold!");
                System.out.println("Choose an option:");
                System.out.println("1. Spend Gold");
                System.out.println("2. Enter the Maze. --WARNING, ONCE ENTERED, YOU CANNOT LEAVE--: ");
                System.out.println("3. Keep Mining");

                String choice = scanner.nextLine();

                switch (choice) {
                    case "1":
                        shop();
                        break;
                    case "2":
                        enterMaze();
                        break;
                    case "3":
                        break;
                    default:
                        System.out.println("Invalid input. Please try again.");
                        continue;
                }
            }

            System.out.println("Press 'a' to mine, or 'b' to visit the shop");
            String input = scanner.nextLine();

            if (input.equals("a")) {
                goldz += goldMultiplier;
            } else if (input.equals("b")) {
                shop();
            } else {
                System.out.println("Invalid input. Please try again.");
            }
        }
    }

    public static void shop() {
        while (true) {
            System.out.println("\n=== Shop Menu ===");
            System.out.println("1. Wooden Pickaxe: +2 gold (25 gold)");
            System.out.println("2. Stone Pickaxe: +5 gold (50 gold)");
            System.out.println("3. Iron Pickaxe: +15 gold (300 gold)");
            System.out.println("4. Buy Sword (150 gold)");
            System.out.println("5. Buy Shield (150 gold)");
            System.out.println("6. Buy Torch (150 gold)");
            System.out.println("Type number to buy, or 'L' to leave.");

            String shopInput = scanner.nextLine();

            switch (shopInput) {
                case "1":
                    if (goldz >= 25) {
                        goldz -= 25;
                        goldMultiplier = 2;
                        System.out.println("You bought the Wooden Pickaxe.");
                    } else {
                        System.out.println("Not enough gold.");
                    }
                    break;

                case "2":
                    if (goldz >= 50) {
                        goldz -= 50;
                        goldMultiplier = 5;
                        System.out.println("You bought the Stone Pickaxe.");
                    } else {
                        System.out.println("Not enough gold.");
                    }
                    break;

                case "3":
                    if (goldz >= 300) {
                        goldz -= 300;
                        goldMultiplier = 15;
                        System.out.println("You bought the Iron Pickaxe.");
                    } else {
                        System.out.println("Not enough gold.");
                    }
                    break;

                case "4":
                    if (goldz >= 150 && !inventory.contains("Sword")) {
                        goldz -= 150;
                        inventory.add("Sword");
                        System.out.println("You bought a Sword.");
                    } else {
                        System.out.println("Not enough gold or you already have it.");
                    }
                    break;

                case "5":
                    if (goldz >= 150 && !inventory.contains("Shield")) {
                        goldz -= 150;
                        inventory.add("Shield");
                        System.out.println("You bought a Shield.");
                    } else {
                        System.out.println("Not enough gold or you already have it.");
                    }
                    break;

                case "6":
                    if (goldz >= 150 && !inventory.contains("Torch")) {
                        goldz -= 150;
                        inventory.add("Torch");
                        System.out.println("You bought a Torch.");
                    } else {
                        System.out.println("Not enough gold or you already have it.");
                    }
                    break;

                case "l":
                case "L":
                    System.out.println("Leaving the shop...");
                    return;

                default:
                    System.out.println("Invalid choice.");
            }

            System.out.printf("Gold: %.0f\n", goldz);
            System.out.println("Inventory: " + inventory);
        }
    }

    public static void enterMaze() {
        mazeEntered = true;
        System.out.println("You step into the Maze...");
        System.out.println("Use W (up), A (left), S (down), D (right) to move.");
        mazeLoop();
    }

    public static void mazeLoop() {
        while (true) {
            System.out.printf("\nYou are at position (%d, %d).\n", mazeX, mazeY);
            System.out.println("Enter direction (W/A/S/D)");
            String move = scanner.nextLine().toLowerCase();

            switch (move) {
                case "w":
                    mazeY++;
                    break;
                case "s":
                    mazeY--;
                    break;
                case "a":
                    mazeX--;
                    break;
                case "d":
                    mazeX++;
                    break;
                default:
                    System.out.println("Invalid direction.");
                    continue;
            }

            if (mazeY > 10 || mazeY < -10 || mazeX > 10 || mazeX < -10) {
                System.out.println("Reached end of map. Spawning back at 0,0...");
                mazeX = 0;
                mazeY = 0;
            }

            randomMazeEvent();

            if (mazeX == 10 && mazeY == -15) {
                System.out.println("--!!YOU WIN!!--");
                System.exit(0);
            }

            if (health <= 0) {
                System.out.println("You have perished in the maze.");
                System.exit(0);
            }
        }
    }

    public static void randomMazeEvent() {
        int event = rand.nextInt(6);  // Now includes case 5

        switch (event) {
            case 0:
            case 1:
                System.out.println("Nothing here. You move cautiously...");
                break;

            case 2:
                System.out.println("You found treasure! +100 gold.");
                goldz += 100;
                break;

            case 3:
                System.out.println("A trap is triggered!");
                if (inventory.contains("Shield")) {
                    System.out.println("Your shield blocks the damage.");
                } else {
                    System.out.println("You take 25 damage!");
                    health -= 25;
                }
                break;

            case 4:
                System.out.println("A wild enemy appears!");
                if (inventory.contains("Sword")) {
                    System.out.println("You defeat it with your sword!");
                } else {
                    System.out.println("You have no weapon! You take 50 damage!");
                    health -= 50;
                }
                break;

            case 5:
                System.out.println("Heavy fog has blurred your vision.");
                if (inventory.contains("Torch")) {
                    System.out.println("Fog cleared.");
                } else {
                    int randomNums = nums.nextInt(5);
                    System.out.println("You got lost.");
                    mazeX = randomNums;
                    mazeY = randomNums;
                }
                break;
        }

        System.out.println("Current Health: " + health);
        System.out.printf("Current Gold: %.0f\n", goldz);
    }
}
