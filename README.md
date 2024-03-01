# Hotel-Billing-System
Developed a Java-based Hotel Billing System in Eclipse, incorporating features for room booking, guest management, billing, and inventory control to streamline hotel operations effectively.                    
package tp;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class HotelBillingSystem {

    private List<Customer> customers;
    private int[] rooms;

    public static void main(String[] args) {
        HotelBillingSystem system = new HotelBillingSystem();
        system.initializeRooms();
        system.initializeCustomers();

        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.println("Enter a customer name (or type 'exit' to quit): ");
            String name = scanner.nextLine();

            if (name.equalsIgnoreCase("exit")) {
                break;
            }

            Customer customer = system.findCustomerByName(name);
            if (customer == null) {
                System.out.println("Customer not found.");
            } else {
                int totalCharge = customer.calculateTotalCharge();

                System.out.println("Customer Name: " + customer.getName());
                System.out.println("Room Number: " + customer.getRoomNumber());
                System.out.println("Number of Days: " + customer.getNumDays());
                System.out.println("Total Charge: $" + totalCharge);

                if (customer instanceof RewardCustomer) {
                    RewardCustomer rewardCustomer = (RewardCustomer) customer;
                    System.out.println("Reward Points: " + rewardCustomer.getRewardPoints());
                }
            }
        }
        scanner.close();
    }

    public void initializeRooms() {
        this.rooms = new int[10];
        for (int i = 0; i < rooms.length; i++) {
            rooms[i] = i + 1;
        }
    }

    public void initializeCustomers() {
        this.customers = new ArrayList<>();
        customers.add(new RegularCustomer("Raghav Bhawsar", 1, 3, 100));
        customers.add(new RegularCustomer("Ashish Neelkanth", 2, 5, 120));
        customers.add(new RewardCustomer("Tushar Patil", 3, 2, 80, 50));
        customers.add(new RewardCustomer("Tony Stark", 4, 4, 90, 70));
    }

    public Customer findCustomerByName(String name) {
        for (Customer customer : customers) {
            if (customer.getName().equals(name)) {
                return customer;
            }
        }
        return null;
    }
}

abstract class Customer {
    protected String name;
    protected int roomNumber;
    protected int numDays;
    protected int roomCharge;

    public Customer(String name, int roomNumber, int numDays, int roomCharge) {
        this.name = name;
        this.roomNumber = roomNumber;
        this.numDays = numDays;
        this.roomCharge = roomCharge;
    }

    public String getName() {
        return name;
    }

    public int getRoomNumber() {
        return roomNumber;
    }

    public int getNumDays() {
        return numDays;
    }

    public abstract int calculateTotalCharge();
}

class RegularCustomer extends Customer {

    public RegularCustomer(String name, int roomNumber, int numDays, int roomCharge) {
        super(name, roomNumber, numDays, roomCharge);
    }

    @Override
    public int calculateTotalCharge() {
        return roomCharge * numDays;
    }
}

class RewardCustomer extends Customer {
    private int rewardPoints;

    public RewardCustomer(String name, int roomNumber, int numDays, int roomCharge, int rewardPoints) {
        super(name, roomNumber, numDays, roomCharge);
        this.rewardPoints = rewardPoints;
    }

    public int getRewardPoints() {
        return rewardPoints;
    }

    @Override
    public int calculateTotalCharge() {
        return (roomCharge * numDays) - rewardPoints;
    }
}
