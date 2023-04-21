import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class StringCompareExample {
    
    public static void main(String[] args) {
        List<String> list1 = Arrays.asList("apple", "banana", "orange", "grape", "peach");
        List<String> list2 = Arrays.asList("apple", "orange", "mango", "pear", "kiwi");
        
        Set<String> set2 = new HashSet<>(list2);
        
        List<String> unmatched = new ArrayList<>();
        
        unmatched.addAll(list1.stream()
                .filter(s -> !set2.contains(s))
                .collect(Collectors.toList()));
        
        unmatched.addAll(list2.stream()
                .filter(s -> !list1.contains(s))
                .collect(Collectors.toList()));
        
        System.out.println("Unmatched strings: " + unmatched);
    }
}
