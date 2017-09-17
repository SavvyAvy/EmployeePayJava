// EmployeePayJava
//Employee payment script
//Avrielle Rouse

import javax.swing.JOptionPane;
import java.util.*;
import java.text.*;
public class Pay
{
   
   private static int sLevel;
    private static float regularPay;
    private static float overtimePay;
    private static float grossPay;
    private static float netPay;
    private static float pRate;
    private static float hWorked;
    private static float overtime;
    private static float regularHours;
    private static float totalDeduc;
    private static String itemizedDeduc;
    private static boolean retirementPlan;
    private static JOptionPane pane;
    private static final float MAX_HOURS = 40.0F;

    public static void main(String[] args)
    {
        pane = new JOptionPane();
        totalDeduc = 0.0F;
        retirementPlan = false;
        itemizedDeduc = "";
        getSkillLevel();
    }

    
    private static void getSkillLevel()
    {
        String input = "";
        boolean isValid = false;
        do
        {
            input = "" + pane.showInputDialog(null, "Please enter employee skill level rating 1-3>>");
            input = input.trim();
            if (!input.equals("null"))
            {
                try
                {
                   
                    sLevel = Integer.parseInt(input);
                    isValid = isSLevelValid();
                }
                catch (Exception e)
                {
                }
                if (!isValid)
                {
                    pane.showMessageDialog(null, "You have entered an invalid skill level - please enter a skill level of 1-3");
                }
            }
            else
            {
                break;
            }
        }
        while (!isValid);
        if (isValid)
        {
            
            setPRate();
            getHoursWorked();
        }
    }

    
    private static void getHoursWorked()
    {
        String input = "";
        boolean isValid = false;
        do
        {
            input = "" + pane.showInputDialog(null, "How many hours has the employee worked?");
            input = input.trim();
            if (!input.equals("null"))
            {
                try
                {
                    
                    hWorked = Float.parseFloat(input);
                    isValid = isHWorkedValid();
                }
                catch (Exception e)
                {
                }
                if (!isValid)
                {
                    pane.showMessageDialog(null, "You have entered an invalid number of hours.");
                }
            }
            else
            {
                break;
            }
        }
        while (!isValid);
        if (isValid)
        {
            calculateHoursWorked();
            getInsuranceOptions();
            calculatePay();
            printResults();
        }
    }

    
     
    private static void getInsuranceOptions()
    {
        int choice = 0;

       
        if (sLevel != 1)
        {
            
            choice = pane.showConfirmDialog(null, "Would employee like to partake in medical insurance?","Insurance", JOptionPane.YES_NO_OPTION);
            if (choice == pane.YES_OPTION) 
            {
                totalDeduc += 32.50F;
                itemizedDeduc += "Medical Insurance:$32.50\n";
            }

         
            choice = pane.showConfirmDialog(null, "Would the employee like to partake in dental insurance?","Insurance", JOptionPane.YES_NO_OPTION);
            if (choice == pane.YES_OPTION)
            {
                totalDeduc += 20.00F;
                itemizedDeduc += "Dental Insurance:$20.00\n";
            }

           
            choice = pane.showConfirmDialog(null, "Does the employee choose longer-term disability?","Insurance", JOptionPane.YES_NO_OPTION);
            if (choice == pane.YES_OPTION)
            {
                totalDeduc += 10.00F;
                itemizedDeduc += "Long-term Disability:$10.00\n";
            }
        }

        if (sLevel == 3)
        {
            choice = pane.showConfirmDialog(null, "Does this employee have a retirement plan?","Retirement", JOptionPane.YES_NO_OPTION);
            retirementPlan = (choice == pane.YES_OPTION);
        }
    }

   
    private static void calculatePay()
    {
        regularPay = regularHours * pRate;
        overtime = overtime * (pRate + (pRate / 2.0F));
        grossPay = regularPay + overtime;

        
        if (retirementPlan)
        {
            float retirementCost = (grossPay * 0.03F);
            totalDeduc += retirementCost;
            itemizedDeduc += "Retirement Plan:$" + retirementCost ;
        }
        netPay = (regularPay + overtime) - totalDeduc;
    }

   
    private static void calculateHoursWorked()
    {
        
        if (hWorked > MAX_HOURS)
        {
            overtime = hWorked - MAX_HOURS;
            regularHours = MAX_HOURS;
        }
        else
        {
            regularHours = hWorked;
            overtime = 0.0F;
        }
    }

    
    private static boolean isSLevelValid()
    {
        boolean isValid;
        switch (sLevel)
        {
            case 1:
            case 2:
            case 3:
            isValid = true;
            break;
            default:
            isValid = false;
            break;
        }
        return isValid;
    }

    
    private static boolean isHWorkedValid()
    {
        return (hWorked > 0.0F);
    }

    
    private static void setPRate()
    {
        switch (sLevel)
        {
            case 1:
            pRate = 17.00F;
            break;
            case 2:
            pRate = 20.00F;
            break;
            case 3:
            pRate = 22.00F;
            break;
            default:
            break;
        }
    }

    
    private static void printResults()
    {
        String message = "";
        message += "Hours worked: " + hWorked + "\n";
        message += "Hourly pay rate:$ " + pRate + "\n";
        message += "Regular pay:$ " + regularPay + "\n";
        message += "Overtime pay:$ " + overtime + "\n";
        message += "\n";
        message += "\n";
        message += "Employee's Gross Pay:$ " + grossPay + "\n";
        
        
                  if (itemizedDeduc != "")
                  {
                        message += "\n";
                        message += "\n";
                        message += "Employee's Deducitons:$";
                        message += itemizedDeduc + "\n";
                  }
                  
                  
        message += "\n";
        message += "\n";
        
        
                  if (grossPay < totalDeduc)
                  {
                        message += " Deductions excede pay \n";
                  }
                     else
                     {
                           message += "Net Income: $" + netPay + "\n";
                     }
                     
        message += "\n";
        message +="\n";
        
        pane.showMessageDialog(null, message);
    }

   

}
