List<String> invalidFruits = Arrays.stream(fruitStrings)
                .filter(fruitStr -> Arrays.stream(Fruit.values())
                        .noneMatch(fruit -> fruit.name().equals(fruitStr)))
                .collect(Collectors.toList());

        // Log the invalid fruits
        invalidFruits.forEach(invalidFruit -> System.out.println("Invalid fruit found: " + invalidFruit));
