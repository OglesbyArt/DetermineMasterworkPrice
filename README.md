/*
 * To change this template, choose Tools | Templates
 * and open the template in the editor.
 */

package oglesby;

import java.util.*;

import java.io.*;

public class DetermineMasterworkPrice
{

private String artistFirstName;
private String artistLastName;
private String titleOfWork;
private String classification;
private Date dateOfWork;
private double height;
private double width;
private String medium;
private String subject;
private double suggestedMaximumPurchasePrice;


public DetermineMasterworkPrice()
{
    executeDetermineMasterworkPrice();
}

    //Desc: Executes the major methods that allow a user to buy a painting.
    // First the user must input values, then the user will view the
    //suggested price. If the user chooses by buy then the bought painting file
    //will be updated, otherwise the user can go back to the main
    //menu
    //Pre: The coefficient of similarity must not be zero, otherwise
    //the user is not interested in buying the painting.
    //Post: The user will have viewed the suggested maximum price for
    //the painting they want to buy. If they chose to buy it, the files are
    //now updated accordingingly.
    public  void executeDetermineMasterworkPrice()
    {
       BoughtPainting bp = new BoughtPainting();

        getValuesFromUser();
       
        System.out.println(calculateMasterworkPrice());
        if ( userBuyChoice(8))
                bp.add();
       
       // else
       // UserInterface.UserInterface();
    }

    //Desc: prompt user to input the artist first name, last name, area of
    //painting, title of work, date of work, medium and subject of painting user
    //wants to buy
    //Post: The artist first name, last name, area of painting, title of
    //work, date of work, medium and subject of painting user wants to buy have
    //values input by the user
    public  void getValuesFromUser()
    {
        try
        {
      //      System.out.println("Enter Artist First name: ");
      //      artistFirstName = UserInterface.getString();

            System.out.println("Enter Artist Last name: ");
            artistLastName= UserInterface.getString();

      //      System.out.println("Enter title of painting: ");
      //      titleOfWork = UserInterface.getString();

            System.out.println("Enter classification of Painting (masterpiece, masterwork, or other): ");
            classification = UserInterface.getString();
            while (!(classification.equalsIgnoreCase("masterpiece")|classification.equalsIgnoreCase("masterwork")|
                    classification.equalsIgnoreCase("other")))
            {
                System.out.println("Classification entered incorrectly. Please enter one of the following mediums: masterpiece, masterwork, or other.");
                classification=UserInterface.getString();
            }

            System.out.println("Enter the date the painting was created (mm/dd/yyyy): ");
            Date tempdate = new Date(UserInterface.getString());
            dateOfWork=tempdate;

            System.out.println("Enter painting medium (oil, watercolor, or other): ");
            medium  = UserInterface.getString();
            while (!(medium.equalsIgnoreCase("oil")|medium.equalsIgnoreCase("watercolor")|
                    medium.equalsIgnoreCase("other")))
            {
                System.out.println("Medium entered incorrectly. Please enter one of the following mediums: oil, watercolor, or other.");
                medium=UserInterface.getString();
            }

            System.out.println("Enter painting subject (portrait, still-life, landscape, or other): ");
            subject =UserInterface.getString();
            while (!(subject.equalsIgnoreCase("portrait")|subject.equalsIgnoreCase("still-life")|
            subject.equalsIgnoreCase("landscape")| subject.equalsIgnoreCase("other")))
            {
                System.out.println("Subject entered incorrectly. Please enter one of the following subjects: portrait, still-life, landscape, or other.");
                subject=UserInterface.getString();
            }

            System.out.println("Enter painting width: ");
            Double tempw=new Double( UserInterface.getString());
            width =tempw;

            System.out.println("Enter painting height: ");
            Double temph=new Double(UserInterface.getString());
            height =temph;
        }

        catch (Exception e)
        {
           System.out.println("Record value entered is not the correct data type. Please re-enter the record: ");
           getValuesFromUser();
        }


    }

    //Desc: constructor for DetermineOtherWorkPrice
    //Post: allows class to set the value of all Painting fields
    // in a record
    public DetermineMasterworkPrice(String fname , String lname, String work, Date dwork,
            String clas, String med, String sub, double h,double w,double max)
    {
		artistFirstName=fname;
		artistLastName=lname;
		titleOfWork=work;
		dateOfWork =dwork;
		classification=clas;
		height=h;
		width=w;
		medium=med;
		subject=sub;
		suggestedMaximumPurchasePrice=max;
    
//may delete this
    }




    //Desc: calculate the price for the Masterwork the user wants to buy
    //Pre: there must be a most similar work and that work must have a an
    //auction purchase price, the user must have entered
    // the date of the work correctly
    //Return: the price of the Masterwork
    public double calculateMasterworkPrice()
    {
        BoughtPainting bp = new BoughtPainting();
        AuctionPainting mp = new AuctionPainting();

    	double auctionPurchasePrice=bp.findPrice(artistLastName, subject, medium, height*width);
    	Date currentDate =new Date();
    	Calendar cal = Calendar.getInstance();
        cal.setTime(dateOfWork);
        int dateOfWorkYear = cal.get(Calendar.YEAR);
      
        cal.setTime(mp.getDateofAuction());
        int dateOfAuctionYear = cal.get(Calendar.YEAR);

        double masterworkPrice = auctionPurchasePrice* Math.pow((1+.085), 2014-dateOfAuctionYear);
    	int c= determineCenturyOfCreation(dateOfWorkYear);
    	if (c==20)
    		return masterworkPrice*.25;
    	else
    		return masterworkPrice*((21-c)/(22-c));
         
         
    }

    //Desc: determine a date’s century of creation which is a value between 12 and 21
    //Pre: the argument must be an int value that has to be in the 4th digits
    //Return: the century of creation for the date
    public  int determineCenturyOfCreation(int i)
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
        //Desc: display a value to the user and the user responds which is returned
    //      as a true or false value
    //Pre: the argument must be a double value
    //Return: a boolean value based on the user’s input
    public static boolean userBuyChoice(double d) 
    {
    	System.out.println("The price is" +d +". Do you want to buy? y/n");
    	String choice=UserInterface.getString();
        while (!choice.equalsIgnoreCase("y")&&!choice.equalsIgnoreCase("n"))
        {
            System.out.println("Please enter the correct format, either y or n");
            System.out.println("The price is" +d +". Do you want to buy? y/n");
            choice=UserInterface.getString();
        }

        if (choice.equalsIgnoreCase("y")) return true;
        else return false;


    }

}



