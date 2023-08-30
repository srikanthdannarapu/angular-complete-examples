private void manageListeners(Boolean stopFlag, String destination) {
    Predicate<ContainerType> listenerPredicate = listenerPredicate(destination);

    containerList.stream()
        .filter(Objects::nonNull)
        .filter(listenerPredicate)
        .forEach(container -> {
            if (stopFlag) {
                container.pause();
                validateAxonListenerStatus(true, container);
            } else if (container.isContainerPaused()) {
                container.resume();
                validateAxonListenerStatus(false, container);
            }
        });
}

