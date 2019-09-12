# Followers

package com.company;

import java.util.HashMap;
import java.util.Scanner;

public class Followers {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        HashMap<String, Integer> usernameLikes = new HashMap<>();
        HashMap<String, Integer> usernameComments = new HashMap<>();
        String input = scanner.nextLine();
        while (!input.equals("Log out")) {
            String[] inputArray = input.split("\\s+");
            String command = inputArray[0];
            if (command.equals("New") && inputArray[1].equals("follower:")) {
                String name = inputArray[2];
                usernameLikes.putIfAbsent(name, 0);
                usernameComments.putIfAbsent(name, 0);
            } else if (command.equals("Like:")) {
                String name = inputArray[1].replace(":", "");
                int like = Integer.parseInt(inputArray[2]);
                if (!usernameLikes.containsKey(name) && !usernameComments.containsKey(name)) {
                    usernameLikes.put(name, like);
                    usernameComments.putIfAbsent(name, 0);
                } else {
                    int oldValue = usernameLikes.get(name);
                    usernameLikes.put(name, oldValue + like);
                }
            } else if (command.equals("Comment:")) {
                String name = inputArray[1];
                if (!usernameLikes.containsKey(name) && !usernameComments.containsKey(name)) {
                    usernameComments.put(name, 1);
                    usernameLikes.putIfAbsent(name, 0);

                } else {
                    int oldValue = usernameComments.get(name);
                    usernameComments.put(name, oldValue + 1);
                }
            } else if (command.equals("Blocked:")) {
                String name = inputArray[1];
                if (!usernameComments.containsKey(name) && !usernameLikes.containsKey(name)) {
                    System.out.println(name + " doesn't exist.");
                } else {
                    usernameComments.remove(name);
                    usernameLikes.remove(name);
                }
            }
            input = scanner.nextLine();
        }
        System.out.println(usernameComments.size() + " followers");

        usernameLikes.
                entrySet().
                stream().
                sorted((f, s) -> {
                    int result = s.getValue().compareTo(f.getValue());
                    if (result == 0) {
                        result = f.getKey().compareTo(s.getKey());
                    }
                    return result;
                }).forEach(
                entry -> System.out.printf("%s: %d%n", entry.getKey(), entry.getValue() + usernameComments.get(entry.getKey()))
        );

    }
}
