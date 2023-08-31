public static void main(String[] args) {
        // Comma-separated string of fruits
        String fruitsString = "MANGO,APPLE,INVALID_FRUIT";

        // Create a map using Java 8 streams
        Map<Fruit, Boolean> hashMap = Arrays.stream(Fruit.values())
                .collect(Collectors.toMap(
                        fruit -> fruit,
                        fruit -> {
                            if (fruitsString.contains(fruit.name())) {
                                return true;
                            } else {
                                System.out.println("Invalid enum value found: " + fruit.name());
                                return false;
                            }
                        }
                ));

        // Print the resulting map
        System.out.println("HashMap: " + hashMap);
    }
