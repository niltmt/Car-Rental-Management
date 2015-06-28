/*
U _____ u   ____     U  ___ u   _       U  ___ u
\| ___"|/U | __")u    \/"_ \/  |"|       \/"_ \/
 |  _|"   \|  _ \/    | | | |U | | u     | | | |
 | |___    | |_) |.-,_| |_| | \| |/__.-,_| |_| |
 |_____|   |____/  \_)-\___/   |_____|\_)-\___/
 <<   >>  _|| \\_       \\     //  \\      \\
(__) (__)(__) (__)     (__)   (_")("_)    (__)
*/

import java.io.*;
import java.util.*;

public class AppMain {

    public static void main(String[] args)  {
        System.out.printf("\nCar Rental Management Application\n\n");
        appControl thisApp = new appControl();
        thisApp.menuMain();
    }
}

class Car implements Serializable {
    String manuf;
    String model;
    int rentRate;
    int rentCur;
    String state;
    ArrayList<Customer> customerList = new ArrayList<>();
}

class Customer implements Serializable {
    String name;
    int dateStart;
    int dateReturn;
}

class CarSD {
    Car obj;
    int percent;
    int id;
}

class databaseInteract {

    ArrayList<Car> list = new ArrayList<>();

    int i=0;

    void loadData() {
        FileInputStream file = null;
        try {
            reset();
            file = new FileInputStream("data/cars.db");
            ObjectInputStream input = new ObjectInputStream(file);
            list = (ArrayList<Car>)input.readObject();
            Car test = list.get(0);
            input.close();
            i = list.size();
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
            System.out.println();
        } catch (ClassNotFoundException e) {
            System.out.println("Error: " + e.getMessage());
            System.out.println();
        } catch (ClassCastException e) {
            System.out.println("Error: " + e.getMessage());
            System.out.println("Warning: Corrupted database was deleted automatically!");
            File reopen = new File("data/cars.db");
            reopen.delete();
        } finally {
            if (file != null)
                try {
                    file.close();
                } catch (IOException x) {}
        }
    }

    void viewAll() {
        try {
            loadData();
            int counter_l = 0;
            if (list.get(counter_l) != null) {
                System.out.println();
                System.out.printf("%-5s. %-30s || %9s || %10s\n", "ID", "Car Model", "Rent Rate", "Status");
                for (int i = 0; i < 64; i++) {
                    System.out.print("=");
                }
                System.out.println();
                while (counter_l < i) {
                    Car currentCar = list.get(counter_l);
                    String currentCarName = list.get(counter_l).manuf + " " + list.get(counter_l).model;
                    System.out.printf("%-5d. %-30s || %9d || %10s\n", counter_l + 1, currentCarName,
                            currentCar.rentRate, currentCar.state);
                    counter_l++;
                }
                for (int i = 0; i < 64; i++) {
                    System.out.print("=");
                }
                System.out.println();
                System.out.println();
            }
        } catch (IndexOutOfBoundsException e) {
            System.out.printf("\nError: Database does not exist or is blank or is corrupted!\n" +
                    "(Please use command 'add' to create the database first or 'delete database' to delete it)\n\n");
        }
    }

    void viewPos(int pos) {
        int pos_r = pos - 1;
        loadData();
        if (list.size() > 0) {
            if (pos > 0 && pos <= i) {
                System.out.printf("\nRecord no.%d - %s\n\n" +
                                "\tRental rate: %d\n" +
                                "\tState: %s\n" +
                                "\tCurrent rental: %d\n",
                        pos, list.get(pos_r).manuf + " " + list.get(pos_r).model, list.get(pos_r).rentRate, list.get(pos_r).state,
                        list.get(pos_r).rentCur);
                viewRentInfo(pos_r);
                System.out.println();
            } else
                System.out.printf("Error: Invalid record ID\n");
        } else
            System.out.printf("Error: Database does not exist or is blank!\n" +
                    "(Please use command 'add' to create the database first or 'delete database' to delete it)\n\n");
    }

    void viewRentInfo(int pos_r) {
        if (!(list.get(pos_r).state.equals("Available"))) {
            System.out.printf("\nRent Information [ID: %d]:\n\n", pos_r + 1);
            int cache_l = 0;
            while (cache_l < list.get(pos_r).rentCur) {
                Customer current = list.get(pos_r).customerList.get(cache_l);
                System.out.printf("\t%d. %s: %d/%d/%d - %d/%d/%d\n", cache_l + 1, current.name,
                        current.dateStart % 100, (current.dateStart / 100) % 100, current.dateStart / 10000,
                        current.dateReturn % 100, (current.dateReturn / 100) % 100, current.dateReturn / 10000);
                cache_l++;
            }
        } else
            System.out.println("\nWarning: There's no rental info for this car at the moment!");
    }

    void deleteAtPos(int pos)  {
        int pos_u = pos - 1;
        loadData();
        if (list.size() > 0) {
            if (pos_u < i && pos_u >= 0) {
                list.remove(pos_u);
                System.out.printf("Warning: Record number %d was deleted successfully!\n\n", pos_u + 1);
            } else {
                System.out.printf("Error: Invalid record ID\n\n");
            }
            write();
        } else
            System.out.printf("Error: Database does not exist or is blank!\n" +
                    "(Please use command 'add' to create the database first or 'delete database' to delete it)\n\n");
    }

    void deleteAll()  {
        loadData();
        System.out.print("Are you sure (y or n)? ");
        String choice = "";
        choice = additionMethod.scanString(choice);
        if (choice.charAt(0) == 'y') {
            list = new ArrayList<>();
            write();
            System.out.printf("Warning: All records were deleted successfully!\n\n");
        } else
            System.out.printf("Warning: No record was deleted!\n\n");
    }

    void deleteDatabase()  {
        try {
            boolean check;
            File current = new File("data/cars.db");
            check = current.delete();
            if (check)
                System.out.printf("Warning: File is deleted successfully!\n\n");
            else {
                System.out.printf("Warning: File has never been existed!\n\n");
            }
        } catch (SecurityException e) {
            System.out.println("Error: Permission denied!");
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }

    void deleteCustomer(int number, int rentCurData)  {
        loadData();
        list.toArray();
        int pos_u = number - 1;
        int end_p = list.get(rentCurData).rentCur - 1;
        if (pos_u <= end_p && pos_u >= 0) {
            list.get(rentCurData).customerList.remove(pos_u);
            list.get(rentCurData).rentCur--;
            write();
        } else {
            System.out.printf("Error: Invalid customer ID!\n");
        }
    }

    void reset() {
        i = 0;
        list = new ArrayList<>();
    }

    void write() {
        try {
            FileOutputStream file = new FileOutputStream("data/cars.db");
            ObjectOutputStream output = new ObjectOutputStream(file);
            output.writeObject(list);
            output.close();
            file.close();
        } catch (IOException ioe) {
            System.out.println("Trouble reading from the file: " + ioe.getMessage());
            System.out.println();
        }
    }

    void addRecord() {
        try {
            loadData();
        } catch (IndexOutOfBoundsException e) {
            i = 0;
        } finally {
            String strCache = "";
            Car currentCar = new Car();
            System.out.print("Type in the Manufacture brand: ");
            currentCar.manuf = additionMethod.scanString(strCache);
            if (currentCar.manuf == null || currentCar.manuf.trim().equals(""))
                System.out.println("Warning: Empty object was entered!");
            System.out.print("Type in the car model: ");
            currentCar.model = additionMethod.scanString(strCache);
            if (currentCar.model == null || currentCar.model.trim().equals(""))
                System.out.println("Warning: Empty object was entered!");
            System.out.print("Type in the state: ");
            strCache = additionMethod.scanString(strCache);
            if (strCache.equalsIgnoreCase("Available") || strCache.equalsIgnoreCase("Avail") || strCache.equalsIgnoreCase("A"))
                currentCar.state = "Available";
            else
                currentCar.state = "N/A";
            System.out.print("Type in the rent rate: ");
            strCache = additionMethod.scanString(strCache);
            while (!additionMethod.isNumeric(strCache) || Integer.parseInt(strCache) < 0) {
                System.out.printf("Error: Total rental rate must be a number greater than 0!\n" +
                        "Please type again: ");
                strCache = additionMethod.scanString(strCache);
            }
            currentCar.rentRate = Integer.parseInt(strCache);
            if (!(currentCar.state.equals("Available"))) {
                strCache = "";
                System.out.print("Type in the number of ongoing rental: ");
                strCache = additionMethod.scanString(strCache);
                while (!additionMethod.isNumeric(strCache) || Integer.parseInt(strCache) > currentCar.rentRate ||
                        Integer.parseInt(strCache) < 0) {
                    if (!additionMethod.isNumeric(strCache) || Integer.parseInt(strCache) < 0) {
                        System.out.printf("Error: Current rental must be a number greater than 0!\n" +
                                "Please type again: ");
                    } else if (Integer.parseInt(strCache) > currentCar.rentRate) {
                        System.out.printf("Error: Ongoing rental number is larger than total rental Rate!\n" +
                                "Please type again: ");
                    }
                    strCache = additionMethod.scanString(strCache);
                }
                currentCar.rentCur = Integer.parseInt(strCache);
                if (currentCar.rentCur == 0 || currentCar.rentRate == 0)
                    currentCar.state = "Available";
                else
                    currentCar.state = "N/A";
                int counter_l = 0;
                while (counter_l < currentCar.rentCur) {
                    int datest = 1;
                    int dateba = 0;
                    Customer currentCustomer = new Customer();
                    System.out.printf("Type in customer %d name: ", counter_l + 1);
                    currentCustomer.name = additionMethod.scanString(strCache);
                    while (dateba < datest) {
                        System.out.printf("Type in Start Date for customer %d: ", counter_l + 1);
                        strCache = additionMethod.scanString(strCache);
                        while (!additionMethod.isValidDate(strCache)) {
                            System.out.printf("Error: Invalid date input ");
                            additionMethod.isValidDate(strCache, true);
                            System.out.printf("Type again the Start date, please: ");
                            strCache = additionMethod.scanString(strCache);
                        }
                        datest = Integer.parseInt(strCache);
                        System.out.printf("Type in Return Date for customer %d: ", counter_l + 1);
                        strCache = additionMethod.scanString(strCache);
                        while (!additionMethod.isValidDate(strCache)) {
                            System.out.printf("Error: Invalid date input ");
                            additionMethod.isValidDate(strCache, true);
                            System.out.printf("Type again the Start date, please: ");
                            strCache = additionMethod.scanString(strCache);
                        }
                        dateba = Integer.parseInt(strCache);
                        if (dateba < datest) {
                            System.out.printf("Error: Return date is larger than Start date! Let's input again\n");
                        }
                    }
                    currentCustomer.dateReturn = dateba;
                    currentCustomer.dateStart = datest;
                    currentCar.customerList.add(currentCustomer);
                    counter_l++;
                }
            } else
                currentCar.rentCur = 0;
            list.add(currentCar);
            write();
            System.out.printf("Warning: Record was added successfully!\n\n");
        }
    }
    void edit(int pos) {
        loadData();
        if (list.size() > 0) {
            int pos_r = pos - 1;
            if (pos > 0 && pos <= i) {
                viewPos(pos);
                System.out.printf("Type in edit sub-option: ");
                String optionLine = "";
                optionLine = additionMethod.scanString(optionLine);
                if (appControl.isLegalCommand(optionLine)) {
                    if (!optionLine.equalsIgnoreCase("cancel")) {
                        ArrayList<String> options = new ArrayList<>();
                        String subOpt = "";
                        additionMethod.stringToArray(optionLine, options);
                        if (options.get(0).equalsIgnoreCase("edit")) {
                            if (options.get(1) != null) {
                                switch (options.get(1)) {
                                    case "car":
                                        System.out.print("Type in new Manufacture: ");
                                        subOpt = additionMethod.scanString(subOpt);
                                        if (subOpt != null && !subOpt.trim().equals(""))
                                            list.get(pos_r).manuf = subOpt;
                                        System.out.print("Type in new car Model: ");
                                        subOpt = additionMethod.scanString(subOpt);
                                        if (subOpt != null && !subOpt.trim().equals(""))
                                            list.get(pos_r).model = subOpt;
                                        break;
                                    case "rentalRate":
                                        System.out.print("Type in new Rental Rate: ");
                                        subOpt = additionMethod.scanString(subOpt);
                                        if (subOpt != null && !subOpt.trim().equals("")) {
                                            while (!additionMethod.isNumeric(subOpt) || Integer.parseInt(subOpt) < list.get(pos_r).rentCur) {
                                                if (!additionMethod.isNumeric(subOpt) || Integer.parseInt(subOpt) < 0) {
                                                    System.out.printf("Error: Rental rate must be a number greater than 0!\n" +
                                                            "Please type again: ");
                                                } else if (Integer.parseInt(subOpt) < list.get(pos_r).rentCur) {
                                                    System.out.printf("Error: Total rental rate is smaller than ongoing rental number!\n" +
                                                            "Please type again: ");
                                                }
                                                subOpt = additionMethod.scanString(subOpt);
                                            }
                                            list.get(pos_r).rentRate = Integer.parseInt(subOpt);
                                        }
                                        break;
                                    case "state":
                                        if (!(list.get(pos_r).state.equals("Available"))) {
                                            System.out.print("Edit contract number: ");
                                            int target = Integer.parseInt(additionMethod.scanString(subOpt));
                                            target--;
                                            if (target >= 0 && target < list.get(pos_r).rentCur) {
                                                int datest = 1;
                                                int dateba = 0;
                                                while (dateba < datest) {
                                                    System.out.print("Type in Start Date: ");
                                                    subOpt = additionMethod.scanString(subOpt);
                                                    if (!subOpt.trim().equals("")) {
                                                        while (!additionMethod.isValidDate(subOpt)) {
                                                            System.out.printf("Error: Invalid date input ");
                                                            additionMethod.isValidDate(subOpt, true);
                                                            System.out.printf("Type again the Start date, please: ");
                                                            subOpt = additionMethod.scanString(subOpt);
                                                        }
                                                        datest = Integer.parseInt(subOpt);
                                                    } else
                                                        datest = list.get(pos_r).customerList.get(target).dateStart;
                                                    System.out.print("Type in Return Date: ");
                                                    subOpt = additionMethod.scanString(subOpt);
                                                    if (!subOpt.trim().equals("")) {
                                                        while (!additionMethod.isValidDate(subOpt)) {
                                                            System.out.printf("Error: Invalid date input ");
                                                            additionMethod.isValidDate(subOpt, true);
                                                            System.out.printf("Type again the Return date, please: ");
                                                            subOpt = additionMethod.scanString(subOpt);
                                                        }
                                                        dateba = Integer.parseInt(subOpt);
                                                    } else
                                                        dateba = list.get(pos_r).customerList.get(target).dateReturn;
                                                    if (dateba < datest) {
                                                        System.out.printf("Error: Return date is larger than Start date! Let's input again\n");
                                                    }
                                                }
                                                list.get(pos_r).customerList.get(target).dateStart = datest;
                                                list.get(pos_r).customerList.get(target).dateReturn = dateba;
                                                System.out.printf("Warning: Information for contract no.%d was updated successfully!\n",
                                                        target + 1);
                                            } else {
                                                System.out.printf("Error: Invalid customer ID: Out of bound!\n");
                                            }
                                        } else {
                                            System.out.printf("Warning: Please use command 'rent [ID]' instead!\n");
                                        }
                                        break;
                                    default:
                                        System.out.printf("Error: Invalid object!\n");
                                        break;
                                }
                            } else
                                System.out.printf("Error: Missing object definition for editing!\n");
                        } else if (options.get(0).equalsIgnoreCase("cancel") || options.get(0).equalsIgnoreCase("finish")) {
                            if (options.get(1) != null && !options.get(1).equals("")) {
                                if (options.get(1).equalsIgnoreCase("all") || (Integer.parseInt(options.get(1)) == 1 &&
                                        list.get(pos_r).rentCur == 1)) {
                                    if (options.get(0).equalsIgnoreCase("finish"))
                                        System.out.printf("Warning: All contracts will be finished. Proceed (y or n)? ");
                                    else
                                        System.out.printf("Warning: All contracts will be canceled. Proceed (y or n)? ");
                                    subOpt = additionMethod.scanString(subOpt);
                                    if (subOpt.charAt(0) == 'y') {
                                        list.get(pos_r).customerList = new ArrayList<>();
                                        list.get(pos_r).state = "Available";
                                        if (options.get(0).equalsIgnoreCase("cancel")) {
                                            System.out.printf("Warning: All contracts were canceled!\n");
                                            list.get(pos_r).rentRate -= list.get(pos_r).rentCur;
                                        } else
                                            System.out.printf("Warning: All contracts were finished!\n");
                                        list.get(pos_r).rentCur = 0;
                                    } else
                                        System.out.printf("Warning: Process is canceled!\n");
                                } else if (additionMethod.isNumeric(options.get(1)) &&
                                        Integer.parseInt(options.get(1)) > 0 &&
                                        Integer.parseInt(options.get(1)) <= list.get(pos_r).rentCur) {
                                    deleteCustomer(Integer.parseInt(options.get(1)), pos_r);
                                    if (options.get(0).equalsIgnoreCase("cancel")) {
                                        System.out.printf("Warning: Contract no.%d was canceled successfully!\n",
                                                Integer.parseInt(options.get(1)));
                                        list.get(pos_r).rentRate--;
                                    } else
                                        System.out.printf("Warning: Contract no.%d was finished successfully!\n",
                                                Integer.parseInt(options.get(1)));
                                } else
                                    System.out.println("Error: Invalid customer ID");
                            } else
                                System.out.printf("Error: Missing argument 'all' or customer ID!\n");
                        } else if (options.get(0).equalsIgnoreCase("delete"))
                            System.out.println("Warning: Please use command delete [...] instead of this sub-option!");
                        else
                            System.out.printf("Error: Invalid edit sub-option!\n");
                    } else
                        System.out.printf("Warning: Process is canceled!\n");
                } else
                    System.out.printf("Error: Invalid edit sub-option!\n");
            } else
                System.out.printf("Error: Invalid record ID\n");
            System.out.println();
        } else
            System.out.printf("Error: Database does not exist or is blank!\n" +
                    "(Please use command 'add' to create the database first)\n\n");
        write();
    }

    void rent(int pos, int back, int start, String name) {
        loadData();
        if (list.size() > 0) {
            int pos_r = pos - 1;
            boolean rentAllow;
            if (list.get(pos_r).state.equals("Available"))
                rentAllow = true;
            else {
                int counter_l = 0;
                rentAllow = true;
                while (counter_l < list.get(pos_r).rentCur) {
                    if ((start >= list.get(pos_r).customerList.get(counter_l).dateStart &&
                            start <= list.get(pos_r).customerList.get(counter_l).dateReturn)
                            || (back >= list.get(pos_r).customerList.get(counter_l).dateStart &&
                            back <= list.get(pos_r).customerList.get(counter_l).dateReturn)) {
                        rentAllow = false;
                        break;
                    }
                    counter_l++;
                }
            }
            if (rentAllow) {
                list.get(pos_r).rentCur++;
                list.get(pos_r).rentRate++;
                list.get(pos_r).state = "N/A";
                Customer current = new Customer();
                current.name = name;
                current.dateStart = start;
                current.dateReturn = back;
                list.get(pos_r).customerList.add(current);
                write();
                System.out.printf("\nWarning: Rented successfully!\n\n");
            } else
                System.out.printf("Error: This car is not available for renting in this period of time!\n\n");
        } else
            System.out.printf("Error: Database does not exist or is blank!\n" +
                    "(Please use command 'add' to create the database first)\n\n");
    }

    void checkDate(int date) {
        try {
            loadData();
            if (list.size() > 0) {
                ArrayList<Integer> available = new ArrayList<>();
                boolean avail;
                int data_c = 0;
                while (data_c < i) {
                    if (list.get(data_c).state.equalsIgnoreCase("Available"))
                        avail = true;
                    else {
                        avail = true;
                        int cus_c = 0;
                        while (cus_c < list.get(data_c).rentCur) {
                            if (date >= list.get(data_c).customerList.get(cus_c).dateStart &&
                                    date <= list.get(data_c).customerList.get(cus_c).dateStart) {
                                avail = false;
                                break;
                            }
                            cus_c++;
                        }
                    }
                    if (avail) {
                        available.add(data_c);
                    }
                    data_c++;
                }
                if (available.size() > 0) {
                    System.out.printf("\nHere are available car ID(s) on %d/%d/%d:\n",
                            date % 100, (date / 100) % 100, date / 10000);
                    System.out.printf("\n%-5s. %-30s || %9s || %10s\n", "ID", "Car Model", "Rent Rate", "Status");
                    for (int j = 0; j < 64; j++) {
                        System.out.print("=");
                    }
                    System.out.println();
                    data_c = 0;
                    while (data_c < available.size()) {
                        System.out.printf("%-5d. %-30s || %9d || %10s\n", available.get(data_c) + 1,
                                list.get(available.get(data_c)).manuf + " " + list.get(available.get(data_c)).model,
                                list.get(available.get(data_c)).rentRate, list.get(available.get(data_c)).state);
                        data_c++;
                    }
                    for (int i = 0; i < 64; i++) {
                        System.out.print("=");
                    }
                    System.out.println("\n");
                } else
                    System.out.println("Warning: There's no car available on that day!");
            } else
                System.out.printf("Error: Database does not exist or is blank!\n" +
                        "(Please use command 'add' to create the database first)\n\n");
        } catch (ClassCastException e) {
            System.out.println("Error: " + e.getMessage());
            System.out.println("Warning: Corrupted database was deleted automatically!");
            File file = new File("data/cars.db");
            file.delete();
        }
    }
    void searchDatabase(String key) {
        try {
            loadData();
            ArrayList<CarSD> searchDatabase = new ArrayList<>();
            if (!additionMethod.isValidDate(key)) {
                for (int data_c = 0; data_c < list.size(); data_c++) {
                    CarSD currentP = new CarSD();
                    currentP.obj = list.get(data_c);
                    currentP.id = data_c;
                    currentP.percent = searchInRecord(list.get(data_c), key);
                    if (currentP.percent < 100)
                        currentP.percent++; //Acceptable uncertainty, up to 0.99%
                    if (currentP.percent > 0) {
                        searchDatabase.add(currentP);
                    }
                }
                if (searchDatabase.size() > 0) {
                    MergerSort searchMethod = new MergerSort();
                    CarSD[] searchDatabaseF = new CarSD[searchDatabase.size()];
                    searchDatabase.toArray(searchDatabaseF);
                    searchMethod.mergeSort(searchDatabaseF);
                    System.out.printf("\n%-5s. %-30s || %9s || %10s\n", "ID", "Car Model", "Rent Rate", "Status");
                    for (int j = 0; j < 64; j++) {
                        System.out.print("=");
                    }
                    System.out.println();
                    for (int i = 0; i < searchDatabaseF.length; i++) {
                        System.out.printf("%-5d. %-30s || %9d || %10s (%d%%)\n", searchDatabaseF[i].id + 1,
                                searchDatabaseF[i].obj.manuf + " " + searchDatabaseF[i].obj.model,
                                searchDatabaseF[i].obj.rentRate, searchDatabaseF[i].obj.state, searchDatabaseF[i].percent);
                    }
                    for (int i = 0; i < 64; i++) {
                        System.out.print("=");
                    }
                    System.out.println();
                } else
                    System.out.println("Warning: No match found!");
            } else
                System.out.printf("Warning: Please use command 'check [validDateFormat]' instead!\n");
        } catch (IndexOutOfBoundsException e) {
            System.out.printf("\nError: Database does not exist or is blank or is corrupted!\n" +
                    "(Please use command 'add' to create the database first or 'delete database' to delete it)\n");
        }
    }
    int searchInRecord(Car target, String key) {
        String[] listOfKeyItem = key.split(" ");
        String targetName = target.manuf + " " + target.model;

        // Search Tool
        String keyItemP;
        int matchPercent = 0;

        for (int key_c = 0; key_c < listOfKeyItem.length; key_c++) {
            keyItemP = listOfKeyItem[key_c];
            ArrayList<Integer> currentItem = new ArrayList<>();
            if (additionMethod.isNumeric(keyItemP)) {
                int doubt = Integer.parseInt(keyItemP);
                //Search in RentRate and RentCur
                if (doubt == target.rentRate || doubt == target.rentCur) {
                    currentItem.add(100);
                }
            }

            //Search in Car model and state
            currentItem.add(additionMethod.searchInString(keyItemP, targetName));
            currentItem.add(additionMethod.searchInString(keyItemP, target.state));

            //Search in customer list
            if (!target.state.equalsIgnoreCase("available")) {
                for (int cus_c = 0; cus_c < target.customerList.size(); cus_c++) {
                    currentItem.add(additionMethod.searchInString(keyItemP, target.customerList.get(cus_c).name));
                }
            }
            //MatchPercent calculation
            matchPercent += Math.ceil(Collections.max(currentItem)*(keyItemP.length()*100/(key.length()-additionMethod.spaceCount(key))))/100;
        }

        return matchPercent;
    }
}

class Instruction {
    int x=0;

    void README()  {
        try {
            x = 0;
            String line;
            FileInputStream file = new FileInputStream("documents/README");
            Scanner input = new Scanner(file, "UTF-8");

            while (input.hasNextLine()) {
                line = input.nextLine();
                System.out.println(line);
            }
            input.close();
            file.close();
        } catch (IOException ioe) {
            System.out.println("Trouble reading from the file: " + ioe.getMessage() + "\n");
        }
    }

    void man(String command) {
        try {
            x = 0;
            String line;
            String fileName = "documents/";
            FileInputStream file = new FileInputStream(fileName + command);
            Scanner input = new Scanner(file, "UTF-8");

            while (input.hasNextLine()) {
                line = input.nextLine();
                System.out.println(line);
            }
            input.close();
            file.close();
        } catch (IOException ioe) {
            System.out.println("Trouble reading from the file: " + ioe.getMessage() + "\n");
        }
    }
}

class appControl {

    databaseInteract database_l = new databaseInteract();
    Instruction appInstruction = new Instruction();
    ArrayList<String> command = new ArrayList<>();

    void menuMain()  {
        System.out.printf("Your command ('about' for more information): ");
        String command_l = "";
        command_l = additionMethod.scanString(command_l);
        if (isLegalCommand(command_l)) {
            commandInput(command_l);
            menuCommandProcess(command);
        } else {
            System.out.printf("Error: Too few or too many arguments (or not 'search')!\n\n");
            menuMain();
        }
    }

    void commandReset() {
        command.clear();
    }

    void commandInput(String commandLine) {
        commandReset();
        additionMethod.stringToArray(commandLine, command);
    }

    void menuCommandProcess(ArrayList<String> argument)  {
        String command_w = argument.get(0);
        switch (command_w) {
            case "delete":
                if (argument.size() > 1) {
                    if (additionMethod.isNumeric(argument.get(1))) {
                        int option = Integer.parseInt(argument.get(1));
                        database_l.viewPos(option);
                        String confirm = "";
                        System.out.print("Warning: Confirm deleting ('y' or 'n')? ");
                        confirm = additionMethod.scanString(confirm);
                        if (confirm.charAt(0) == 'y')
                            database_l.deleteAtPos(option);
                        else
                            System.out.println("Warning: Process is canceled!\n");
                    } else {
                        String option = argument.get(1);
                        if (option.equalsIgnoreCase("all")) {
                            database_l.deleteAll();
                        } else if (option.equalsIgnoreCase("database")) {
                            database_l.deleteDatabase();
                        } else {
                            System.out.printf("Error: Invalid argument! (Did you mean 'delete all'?)\n\n");
                        }
                    }
                } else
                    System.out.printf("Error: Missing argument!\n\n");
                menuMain();
                break;
            case "view":
                if (argument.size() > 1) {
                    if (argument.get(1).equals("all"))
                        database_l.viewAll();
                    else if (additionMethod.isNumeric(argument.get(1))) {
                        int option = Integer.parseInt(argument.get(1));
                        database_l.viewPos(option);
                    } else
                        System.out.printf("Error: Invalid command option!\n\n");
                } else
                    System.out.printf("Error: Missing argument!\n\n");
                menuMain();
                break;
            case "add":
                database_l.addRecord();
                if (argument.size() == 2) {
                    if (additionMethod.isNumeric(argument.get(1))) {
                        for (int count = 1; count < Integer.parseInt(argument.get(1)); count++)
                            database_l.addRecord();
                    } else
                        System.out.printf("Error: Invalid argument (none or only [number] accepted)!\n\n");
                } else if (argument.size() > 2)
                    System.out.printf("Error: Too many arguments!\n\n");
                menuMain();
                break;
            case "edit":
                if (argument.size() > 1) {
                    if (additionMethod.isNumeric(argument.get(1))) {
                        int pos = Integer.parseInt(argument.get(1));
                        database_l.edit(pos);
                    } else
                        System.out.printf("Error: Invalid record ID\n");
                }else
                    System.out.printf("Error: Missing record ID!\n\n");
                menuMain();
                break;
            case "rent":
                if (argument.size() > 1) {
                    if (additionMethod.isNumeric(argument.get(1))) {
                        database_l.viewPos(Integer.parseInt(argument.get(1)));
                        int carID = Integer.parseInt(argument.get(1));
                        String strCache = "";
                        System.out.print("Type in Customer Name: ");
                        String strName = additionMethod.scanString(strCache);
                        int datest = 1;
                        int dateba = 0;
                        while (dateba < datest) {
                            System.out.print("Type in Start Date: ");
                            strCache = additionMethod.scanString(strCache);
                            while (!additionMethod.isValidDate(strCache)) {
                                System.out.printf("Error: Invalid date input ");
                                additionMethod.isValidDate(strCache,true);
                                System.out.printf("Type again the Start date, please: ");
                                strCache = additionMethod.scanString(strCache);
                            }
                            datest = Integer.parseInt(strCache);
                            System.out.print("Type in Return Date: ");
                            strCache = additionMethod.scanString(strCache);
                            while (!additionMethod.isValidDate(strCache, true)) {
                                System.out.printf("Error: Invalid date input ");
                                additionMethod.isValidDate(strCache,true);
                                System.out.printf("Type again the Start date, please: ");
                                strCache = additionMethod.scanString(strCache);
                            }
                            dateba = Integer.parseInt(strCache);
                            if (dateba < datest) {
                                System.out.printf("Error: Return date is larger than Start date! Let's input again\n");
                            }
                        }
                        database_l.rent(carID, dateba, datest, strName);
                    } else
                        System.out.printf("Error: Invalid record ID!\n");
                } else
                    System.out.printf("Error: Missing record ID!\n\n");
                menuMain();
                break;
            case "check":
                if (argument.size() > 1) {
                    if (additionMethod.isValidDate(argument.get(1)))
                        database_l.checkDate(Integer.parseInt(argument.get(1)));
                    else {
                        System.out.print("Error: Invalid date input ");
                        additionMethod.isValidDate(argument.get(1),true);
                    }
                } else
                    System.out.printf("Error: Missing date object to check!\n\n");
                menuMain();
                break;
            case "search":
                if (argument.size() < 2)
                    System.out.printf("Error: Missing search item!\n");
                else {
                    String searchItem = argument.get(1);
                    if (argument.size() > 2) {
                        for (int i = 2; i < argument.size(); i++) {
                            searchItem += " ";
                            searchItem += argument.get(i);
                        }
                    }
                    database_l.searchDatabase(searchItem);
                }
                System.out.println();
                menuMain();
                break;
            case "about":
                appInstruction.README();
                menuMain();
                break;
            case "man":
                if (argument.size() > 1) {
                    appInstruction.man(argument.get(1));
                } else
                    System.out.printf("Error: Missing command name!\n\n");
                menuMain();
                break;
            case "exit":
                System.out.printf("Warning: Application exited successfully!\nHave a nice day!\n");
                appInstruction.man("ebolo");
                break;
            default:
                System.out.printf("Error: Unrecognized command!\n\n");
                menuMain();
                break;
        }
    }

    public static boolean isLegalCommand(String inputCommand) {
        int spaceCount = 0;
        for (int i = 0; i < inputCommand.length(); i++) {
            if (inputCommand.charAt(i) == ' ')
                spaceCount++;
            if (spaceCount > 2)
                break;
        }
        if (spaceCount <= 1 || inputCommand.substring(0,6).equalsIgnoreCase("search") && inputCommand.length() > 6)
            return true;
        else
            return false;
    }
}

class additionMethod {
    public static boolean isNumeric(String str)
    {
        return str.matches("-?\\d+(\\.\\d+)?");  //match a number with optional '-' and decimal.
    }

    public static String scanString(String s) {
        try {
            BufferedReader bufferRead = new BufferedReader(new InputStreamReader(System.in));
            s = bufferRead.readLine();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return s;
    }

    public static void stringToArray(String str, ArrayList<String> strArray) {
        String[] strItem = str.split(" ");
        for (int i = 0; i < strItem.length; i++) {
            strArray.add(strItem[i]);
        }
    }

    public static boolean isValidDate(String date) {
        boolean result = false;
        if (isNumeric(date)) {
            if (date.length() == 8) {
                int dateInt = Integer.parseInt(date);
                int dd = dateInt % 100;
                int mm = (dateInt / 100) % 100;
                int yyyy = dateInt / 10000;
                if (yyyy > 2015 || (yyyy == 2015 && mm > 6)) {
                    if (mm > 0 && mm <= 12) {
                        switch (mm) {
                            case 4:
                            case 6:
                            case 9:
                            case 11:
                                if (dd <= 30 && dd > 0)
                                    result = true;
                                break;
                            case 2:
                                if (((yyyy % 4 == 0) && (yyyy % 100 != 0)) || (yyyy % 400 == 0))
                                    if (dd <= 29 && dd > 0)
                                        result = true;
                                    else if (dd <= 28 && dd > 0)
                                        result = true;
                                break;
                            default:
                                if (dd <= 31 && dd > 0)
                                    result = true;
                                break;
                        }
                    }
                }
            }
        }
        return result;
    }

    public static boolean isValidDate(String date, boolean debug) {
        boolean result = false;
        if (isNumeric(date)) {
            if (date.length() == 8) {
                int dateInt = Integer.parseInt(date);
                int dd = dateInt % 100;
                int mm = (dateInt / 100) % 100;
                int yyyy = dateInt / 10000;
                if (yyyy > 2015 || (yyyy == 2015 && mm > 6)) {
                    if (mm > 0 && mm <= 12) {
                        switch (mm) {
                            case 4:
                            case 6:
                            case 9:
                            case 11:
                                if (dd <= 30 && dd > 0)
                                    result = true;
                                break;
                            case 2:
                                if (((yyyy % 4 == 0) && (yyyy % 100 != 0)) || (yyyy % 400 == 0))
                                    if (dd <= 29 && dd > 0)
                                        result = true;
                                    else if (dd <= 28 && dd >0)
                                        result = true;
                                break;
                            default:
                                if (dd <= 31 && dd > 0)
                                    result = true;
                                break;
                        }
                    } else
                        if (debug)
                            System.out.printf("(Invalid month input!)\n\n");
                } else
                    if (debug)
                        System.out.printf("(We haven't have Time machine yet!)\n\n");
            } else
                if (debug)
                    System.out.printf("(Un-finished time format (yyyymmdd)!)\n\n");
        } else
            if (debug)
                System.out.printf("(Invalid date format: character detected!)\n\n");
        return result;
    }

    public static int searchInString(String key, String target) {
        int match_percent = 0;
        String keyP;
        for (int key_c = key.length(); key_c > 0; key_c--) {
            keyP = key.substring(0,key_c);
            if (target.toLowerCase().contains(keyP.toLowerCase())) {
                match_percent = keyP.length()*100/key.length();
                break;
            }
        }
        if (match_percent > 50)
            return match_percent;
        else
            return 0;
    }

    public static  int spaceCount(String stringInput) {
        int space = 0;
        for (int i = 0; i < stringInput.length(); i++) {
            if (stringInput.charAt(i) == ' ')
                space++;
        }
        return space;
    }
}

class MergerSort {
    void mergeSort(CarSD[] a) {
        CarSD[] tmp = new CarSD[a.length];
        mergeSort(a, tmp, 0, a.length - 1);
    }

    void mergeSort(CarSD[] a, CarSD[] tmp, int left, int right) {
        if( left < right ) {
            int center = (left + right) / 2;
            mergeSort(a, tmp, left, center);
            mergeSort(a, tmp, center + 1, right);
            merge(a, tmp, left, center + 1, right);
        }
    }

    void merge(CarSD[] a, CarSD[] tmp, int left, int right, int rightEnd ) {
        int leftEnd = right - 1;
        int k = left;
        int num = rightEnd - left + 1;

        while(left <= leftEnd && right <= rightEnd)
            if(a[left].percent >= a[right].percent)
                tmp[k++] = a[left++];
            else
                tmp[k++] = a[right++];

        while(left <= leftEnd)    // Copy rest of first half
            tmp[k++] = a[left++];

        while(right <= rightEnd)  // Copy rest of right half
            tmp[k++] = a[right++];

        // Copy tmp back
        for(int i = 0; i < num; i++, rightEnd--)
            a[rightEnd] = tmp[rightEnd];
    }
}
