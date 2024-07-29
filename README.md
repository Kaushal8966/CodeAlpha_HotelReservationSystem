# CodeAlpha_HotelReservationSystem
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

// Enum for Room Categories
enum RoomCategory {
    STANDARD, DELUXE, SUITE
}

// Room Class
class Room {
    private int roomNumber;
    private RoomCategory category;
    private boolean isAvailable;

    public Room(int roomNumber, RoomCategory category) {
        this.roomNumber = roomNumber;
        this.category = category;
        this.isAvailable = true;
    }

    public int getRoomNumber() {
        return roomNumber;
    }

    public RoomCategory getCategory() {
        return category;
    }

    public boolean isAvailable() {
        return isAvailable;
    }

    public void setAvailable(boolean available) {
        isAvailable = available;
    }

    @Override
    public String toString() {
        return "Room " + roomNumber + " (" + category + ")";
    }
}

// Reservation Class
class Reservation {
    private Room room;
    private String guestName;

    public Reservation(Room room, String guestName) {
        this.room = room;
        this.guestName = guestName;
    }

    public Room getRoom() {
        return room;
    }

    public String getGuestName() {
        return guestName;
    }

    @Override
    public String toString() {
        return "Reservation for " + guestName + " in " + room;
    }
}

// Hotel Class
class Hotel {
    private List<Room> rooms;
    private Map<Room, Reservation> reservations;

    public Hotel() {
        rooms = new ArrayList<>();
        reservations = new HashMap<>();
    }

    public void addRoom(Room room) {
        rooms.add(room);
    }

    public List<Room> searchAvailableRooms(RoomCategory category) {
        return rooms.stream()
                .filter(room -> room.getCategory() == category && room.isAvailable())
                .collect(Collectors.toList());
    }

    public Room bookRoom(int roomNumber, String guestName) {
        for (Room room : rooms) {
            if (room.getRoomNumber() == roomNumber && room.isAvailable()) {
                room.setAvailable(false);
                Reservation reservation = new Reservation(room, guestName);
                reservations.put(room, reservation);
                return room;
            }
        }
        return null; // Room not available
    }

    public Reservation viewReservation(int roomNumber) {
        for (Room room : reservations.keySet()) {
            if (room.getRoomNumber() == roomNumber) {
                return reservations.get(room);
            }
        }
        return null; // Reservation not found
    }
}

// Payment Processor Class
class PaymentProcessor {
    public boolean processPayment(double amount) {
        // Mock payment processing
        System.out.println("Processing payment of $" + amount);
        return true; // Assume payment is successful
    }
}

// Main Class
public class HotelReservationSystem {
    public static void main(String[] args) {
        Hotel hotel = new Hotel();
        hotel.addRoom(new Room(101, RoomCategory.STANDARD));
        hotel.addRoom(new Room(102, RoomCategory.DELUXE));
        hotel.addRoom(new Room(103, RoomCategory.SUITE));

        // Search for available rooms
        System.out.println("Available Standard Rooms: " + hotel.searchAvailableRooms(RoomCategory.STANDARD));

        // Book a room
        Room bookedRoom = hotel.bookRoom(103, "john Doe");
        if (bookedRoom != null) {
            System.out.println("Booked Room: " + bookedRoom);
        } else {
            System.out.println("Room not available.");
        }

        // View reservation
        Reservation reservation = hotel.viewReservation(101);
        if (reservation != null) {
            System.out.println("Reservation Details: " + reservation);
        } else {
            System.out.println("No reservation found.");
        }

        // Process payment
        PaymentProcessor paymentProcessor = new PaymentProcessor();
        paymentProcessor.processPayment(100.0);
    }
}
