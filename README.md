List<String> destinationNames = customizersList.stream()
    .map(customizer -> customizer.configure(null, null, null))
    .map(AbstractMessageListenerContainer::getDestinationName)
    .collect(Collectors.toList());
