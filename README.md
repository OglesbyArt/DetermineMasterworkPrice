DetermineMasterworkPrice
========================
import java.util.*;

public class DetermineMasterworkPrice
{

private String artistFirstName;
private String artistLastName;
private String titleOfWork;
private String classification;
//private date dateOfWork;
private double height;
private double width;
private String medium;
private String subject;
private double suggestedMaximumPurchasePrice;



    //Desc: constructor for DetermineOtherWorkPrice
	//Post: allows class to set the value of all Painting fields 
    // in a record
    public DetermineMasterworkPrice(String fname, String lname, String work, String clas, double h,double w, String med, String sub, double max)
    {
		artistFirstName=fname;
		artistLastName=lname;
		titleOfWork=work;
	//	dateOfWork=dwork
		classification=clas;
		height=h;
		width=w;
		medium=med;
		subject=sub;
		suggestedMaximumPurchasePrice=max;
    }




    //Desc: Looks through the bought file for paintings by the same artist
    //and computes the coefficient of similarity for that painting. 
    //It will return the purchase price for the painting with the 
    //highest coefficient of similarity
    //Pre: There must be another painting by the same artist in 
    //the painting file
    //Return: The price of the painting with the highest coefficient
    //of similarity
    public static double findLargestCoefficientofSimilarity()
    {

    	double coeff=0;
    	double dummycoeff=0;
    	int score1=0;
    	int score2=0;
    	double largerarea=0;
    	double smallerArea=0;
    	double suggestMaxPrice=0;

       // create  determineMostSimilarWork object

     /*   loop through array for bought paintings objects
        {
        	find(artistname)
        	score1=computeScore(getmedium(), medium)
    		score2=computeScore(getsubject(), subject)
        	smallerArea=width*height 
        	largerArea=getWidth()*getHeight() 
    		largerarea=compareReturnLarger(largerarea, smallerArea)
    		smallerArea=compareReturnSmaller(largerarea, smallerArea)
			dummycoeff=computeCoefficientofSimilarity(score1, 
                        smallerArea, largerarea)
			if (coeff<dummycoeff)
			{
				coeff=dummycoeff
				suggestMaxPrice=maxpurchaseprice
			}
    	}*/
    	return suggestMaxPrice;
    }

    
    
    //Desc: calculate the price for the Masterwork the user wants to buy
    //Pre: there must be a most similar work and that work must have a an 
    //auction purchase price, the user must have entered 
    // the date of the work correctly
    //Return: the price of the Masterwork 
    public static double calculateMasterworkPrice()
    {
    	double auctionPurchasePrice=findLargestCoefficientofSimilarity();
    	int currentYear=0; //getDate()
    	int auctionYear = 2000;//dateOfWork
    	double masterworkPrice=auctionPurchasePrice;//*(8.5)^((1/auctionYear)-currentYear) + 1
    	int c= determineCenturyOfCreation(auctionYear);
    	if (c==20)
    		return masterworkPrice*.25;
    	else
    		return masterworkPrice*((21-c)/(22-c));
    }
        
    //Desc: determine a date’s century of creation which is a value between 12 and 21
    //Pre: the argument must be an int value that has to be in the 4th digits
    //Return: the century of creation for the date
    public static int determineCenturyOfCreation(int i)
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
}
 

