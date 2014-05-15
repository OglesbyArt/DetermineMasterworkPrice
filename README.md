/*
 * To change this template, choose Tools | Templates
 * and open the template in the editor.
 */

package oglesby;

import java.util.*;

import java.text.*;


class DetermineMasterworkPrice
{




     //Desc: constructor for DetermineMasterworkPrice
     //Post: Creates a new DetermineMasterworkPrice
     public DetermineMasterworkPrice()
     {
    	executeDetermineMasterworkPrice();
     }

    //Desc: Executes the major methods that allow a user to buy a painting.
    //  First the user must input values, then the user will view the
    //  suggested price. If the user chooses by buy then the bought painting
    //  file will be updated, otherwise the user can go back to the main
    //  menu
    //Pre: The coefficient of similarity must not be zero, otherwise
    //  the user is not interested in buying the painting.
    //Post: The user will have viewed the suggested maximum price for
    //  the painting they want to buy. If they chose to buy it, the files are
    //  now updated accordingingly.
    public static void executeDetermineMasterworkPrice()
    {
       BoughtPainting bp = new BoughtPainting();

        bp.readInRecord();
        double area =bp.getHeight()*bp.getWidth();
        boolean choice=false;
        double suggestedMaximumPurchasePrice=calculateMasterworkPrice(bp.getArtistLastName(), bp.getSubject(), bp.getMedium(),area, bp.getDateofWork() );

       if (suggestedMaximumPurchasePrice==0)
        {
            System.out.println("A painting with a coefficient of similarity that is zero cannot be bought.");
            choice=false;
        }
        else choice = userBuyChoice(suggestedMaximumPurchasePrice);
        if ( choice)
        {

            bp.setSuggestedMaximumPurchasePrice(suggestedMaximumPurchasePrice);
            bp.addRecentlyBought();
            System.out.println("");
            System.out.println("Press <ENTER> to return to the Buy menu.");
            UserInterface.pressEnter();
        }

        else
        {
            System.out.println("");
            System.out.println("Press <ENTER> to return to the Buy menu.");
             UserInterface.pressEnter();
        }

    }

    //Desc: calculate the price for the Masterwork the user wants to buy
    //Pre: there must be a most similar work and that work must have a an
    //  auction purchase price, the user must have entered
    //  the date of the work correctly
    //Return: the price of the Masterwork
    public static double calculateMasterworkPrice(String artistLastName, String subject, String medium, double area, Date dateOfWork)
    {

       DetermineMostSimilarWork ap = new DetermineMostSimilarWork();

    	double auctionPurchasePrice=ap.findPrice(artistLastName, medium, subject, area);
        Date auctiondate;

        if (auctionPurchasePrice==0)
        {
            return 0;
        }

    	Date currentDate =new Date();
    	Calendar cal = Calendar.getInstance();
        cal.setTime(dateOfWork);
        double dateOfWorkYear = cal.get(Calendar.YEAR);


        auctiondate=ap.findDate(artistLastName,medium, subject,  area);
        cal.setTime(auctiondate);
        int dateOfAuctionYear = cal.get(Calendar.YEAR);


        cal.setTime(currentDate);
        int currentDateYear = cal.get(Calendar.YEAR);


        double masterworkPrice = auctionPurchasePrice* Math.pow((1+.085), currentDateYear-dateOfAuctionYear);


   	double c= determineCenturyOfCreation(dateOfWorkYear);
    	if (c==20)
    		return masterworkPrice*.25;
    	else
        {

            double constant =((21-c)/(22-c));
            return masterworkPrice*constant ;
            
        }
    		

    }

    //Desc: determine a date’s century of creation which
    //  is a value between 12 and 21
    //Pre: the argument must be an int value that has to be in the 4th digits
    //Return: the century of creation for the date
    public  static double determineCenturyOfCreation(double i)
    {
        int century;
        if (i<1100) century=0;
        else if (i<1200)century=12;
        else if (i<1300 )century =13;
        else if (i<1400) century =14;
        else if (i<1500 )century =15;
        else if (i<1600 )century =16;
        else if (i<1700) century =17;
        else if (i<1800) century =18;
        else if (i<1900) century =19;
        else if (i<2000) century =20;
        else century=21;
        return century;
    }

    //Desc: display a value to the user and the user responds
    //  which is returned as a true or false value
    //Pre: the argument must be a double value
    //Return: a boolean value based on the user’s input
    public  static boolean userBuyChoice(double d)
    {
    	DecimalFormat dec = new DecimalFormat("#.##");
        DecimalFormat comma = new DecimalFormat("#,###.00");
        double doubleComma =Double.parseDouble(dec.format(d));
        System.out.println("The Suggested Maximum Purchase Price is $" +comma.format(doubleComma)+". Do you want to buy? y/n");
    	String choice=UserInterface.getString();
        while (!choice.equalsIgnoreCase("y")&&!choice.equalsIgnoreCase("n"))
        {
            System.out.println("Please enter the correct format, either y or n");
            System.out.println("The Suggested Maximum Purchase Price is $" +comma.format(doubleComma) +". Do you want to buy? y/n");
            choice=UserInterface.getString();
        }

        if (choice.equalsIgnoreCase("y")) return true;
        else return false;
    }

}



