# yonca
Technical Questionnaire - Junior Front End SW Developer

------



using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;

namespace Rextester
{
    public static class Program
    {
        public static Dictionary<int, bool> PrimeCache = new Dictionary<int, bool>();
        public static void Main(string[] args)
        {
            var result = GetInput()
            .TransformInputToArray()
            .TransformTo2Darray()
            .ResetAllPrimeNumbers()
            .WalkThroughTheNode();

        String c=result.ToString();
        Console.WriteLine(c);
        Console.Read();
 
        }
        
        
          private static string GetInput()
    {
        const string input = @"           *1
     *8 4
    2 *6 9
   8 5 *9 3";
        return input;
    }

    
    private static string[] TransformInputToArray(this string input)
    {
        return input.Split(new[] {Environment.NewLine}, StringSplitOptions.RemoveEmptyEntries);
    }


    private static int[,] TransformTo2Darray(this string[] arrayOfRowsByNewlines)
    {
        var tableHolder = new int[arrayOfRowsByNewlines.Length, arrayOfRowsByNewlines.Length + 1];

        for (var row = 0; row < arrayOfRowsByNewlines.Length; row++)
        {
            var eachCharactersInRow = arrayOfRowsByNewlines[row].ExtractNumber();

            for (var column = 0; column < eachCharactersInRow.Length; column++)
                tableHolder[row, column] = eachCharactersInRow[column];
        }
        return tableHolder;
    }

  
    private static int[] ExtractNumber(this string rows)
    {
        return
            Regex
                .Matches(rows, "[0-9]+")
                .Cast<Match>()
                .Select(m => int.Parse(m.Value)).ToArray();
    }

    private static int[,] ResetAllPrimeNumbers(this int[,] tableHolder)
    {
        var length = tableHolder.GetLength(0);
        for (var i = 0; i < length; i++)
        {
            for (var j = 0; j < length; j++)
            {
                if (tableHolder[i, j] == 0) continue;
                if (IsPrime(tableHolder[i, j]))
                    tableHolder[i, j] = 0;
               
            }
        }
        return tableHolder;
    }

    
    private static int WalkThroughTheNode(this int[,] tableHolder)
    {
        var tempresult = tableHolder;
        var length = tableHolder.GetLength(0);
        //length=length+1;
        Console.WriteLine(length);
        var count=0;
      
      
        for (var i =length-2; i >= 0 ; i--)
        {
            for (var j = 0; j < length; j++)
            {
               
                
                
                
                if (tableHolder[i, j] == 0) continue;
                
                else tableHolder[i, j]= tempresult[i, j] + Math.Max(tempresult[i + 1, j],tempresult[i + 1, j + 1]); count++;
      
            }
        }  
        Console.WriteLine(count);
        return tableHolder[0, 0];
    }

    
    public static bool IsPrime(this int number)
    {
     
        if (PrimeCache.ContainsKey(number))
        {
            bool value;
            PrimeCache.TryGetValue(number, out value);
            return value;
        }
        if ((number & 1) == 0)
        {
            if (number == 2)
            {
                if (!PrimeCache.ContainsKey(number)) PrimeCache.Add(number, true);
                return true;
            }
            if (!PrimeCache.ContainsKey(number)) PrimeCache.Add(number, false);
            return false;
        }

        for (var i = 3; i*i <= number; i += 2)
        {
            if (number%i == 0)
            {
                if (!PrimeCache.ContainsKey(number)) PrimeCache.Add(number, false);
                return false;
            }
        }
        var check = number != 1;
        if (!PrimeCache.ContainsKey(number)) PrimeCache.Add(number, check);
        return check;
    }
        
        
    }
  

}
