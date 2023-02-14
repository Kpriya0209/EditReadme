# EditReadmeimport java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

public class Flentas {
    public static void main(String[] args) {
            String[] callLog = {
                    "9898989898, 8989898989, 00:05:23",
                    "1898989898, 9989898989, 01:00:23",
                    "2898989898, 7989898989, 00:00:21",
                    "2898989898, 6989898989, 00:00:23",
                    "4898989898, 5989898989, 00:01:23",
                    "5898989898, 4989898989, 00:05:55",
                    "5898989898, 3989898989, 00:00:23",
                    "7898989898, 2989898989, 00:02:23",
                    "8898989898, 1189898989, 00:05:20",
                    "1098989898, 8459898989, 00:00:23",
                    "1898989898, 8967898989, 00:00:40",
                    "2898989898, 8989898989, 00:05:23"
            };

            Set<String> distinctFromNumbers = new HashSet<>();
            Set<String> freePlanUsers = new HashSet<>();
            Map<String, Integer> callDurationByFromNumber = new HashMap<>();
            int totalIncome = 0;

            for (String call : callLog) {
                String[] callDetails = call.split(", ");
                String fromNumber = callDetails[0];
                String toNumber = callDetails[1];
                String duration = callDetails[2];

                // 1. Find the distinct From Numbers for a day
                distinctFromNumbers.add(fromNumber);

                // 2. Find the distinct From Numbers who used the Free Plan
                if (duration.compareTo("00:01:00") < 0) {
                    freePlanUsers.add(fromNumber);
                }

                // 3. Find the total call duration with respect to From Number
                int callDurationInSeconds = getDurationInSeconds(duration);
                int totalDuration = callDurationByFromNumber.getOrDefault(fromNumber, 0) + callDurationInSeconds;
                callDurationByFromNumber.put(fromNumber, totalDuration);

                // 4. Find the total income for a day
                if (callDurationInSeconds > 60) {
                    int callDurationInMinutes = (int) Math.ceil(callDurationInSeconds / 60.0);
                    int callCost = callDurationInMinutes * 30;
                    totalIncome += callCost;
                }
            }

            System.out.println("Distinct From Numbers: " + distinctFromNumbers);
            System.out.println("Free Plan Users: " + freePlanUsers);
            System.out.println("Total Call Duration by From Number: " + callDurationByFromNumber);
            System.out.println("Total Income for a day: " + totalIncome);
        }

        private static int getDurationInSeconds(String duration) {
            String[] parts = duration.split(":");
            int hours = Integer.parseInt(parts[0]);
            int minutes = Integer.parseInt(parts[1]);
            int seconds = Integer.parseInt(parts[2]);
            return (hours * 60 + minutes) * 60 + seconds;
        }
    }


