using System;

class FCFS
{
    static void Main()
    {
        Console.Write("Enter number of processes: ");
        int n = int.Parse(Console.ReadLine());

        int[] bt = new int[n];
        int[] wt = new int[n];
        int[] tat = new int[n];

        Console.WriteLine("Enter burst times:");
        for (int i = 0; i < n; i++)
            bt[i] = int.Parse(Console.ReadLine());

        wt[0] = 0;
        for (int i = 1; i < n; i++)
            wt[i] = wt[i - 1] + bt[i - 1];

        Console.WriteLine("\nP\tBT\tWT\tTAT");
        for (int i = 0; i < n; i++)
        {
            tat[i] = bt[i] + wt[i];
            Console.WriteLine($"P{i + 1}\t{bt[i]}\t{wt[i]}\t{tat[i]}");
        }
    }
}
using System;

class SJF
{
    static void Main()
    {
        Console.Write("Enter number of processes: ");
        int n = int.Parse(Console.ReadLine());

        int[] bt = new int[n];
        int[] wt = new int[n];
        int[] tat = new int[n];

        Console.WriteLine("Enter burst times:");
        for (int i = 0; i < n; i++)
            bt[i] = int.Parse(Console.ReadLine());

        Array.Sort(bt);

        wt[0] = 0;
        for (int i = 1; i < n; i++)
            wt[i] = wt[i - 1] + bt[i - 1];

        Console.WriteLine("\nP\tBT\tWT\tTAT");
        for (int i = 0; i < n; i++)
        {
            tat[i] = bt[i] + wt[i];
            Console.WriteLine($"P{i + 1}\t{bt[i]}\t{wt[i]}\t{tat[i]}");
        }
    }
}
using System;

class PriorityNP
{
    static void Main()
    {
        Console.Write("Enter number of processes: ");
        int n = int.Parse(Console.ReadLine());

        int[] bt = new int[n];
        int[] pr = new int[n];
        int[] wt = new int[n];
        int[] tat = new int[n];

        Console.WriteLine("Enter burst time and priority:");
        for (int i = 0; i < n; i++)
        {
            bt[i] = int.Parse(Console.ReadLine());
            pr[i] = int.Parse(Console.ReadLine());
        }

        for (int i = 0; i < n - 1; i++)
            for (int j = i + 1; j < n; j++)
                if (pr[i] > pr[j])
                {
                    (pr[i], pr[j]) = (pr[j], pr[i]);
                    (bt[i], bt[j]) = (bt[j], bt[i]);
                }

        wt[0] = 0;
        for (int i = 1; i < n; i++)
            wt[i] = wt[i - 1] + bt[i - 1];

        Console.WriteLine("\nP\tBT\tPR\tWT\tTAT");
        for (int i = 0; i < n; i++)
        {
            tat[i] = bt[i] + wt[i];
            Console.WriteLine($"P{i + 1}\t{bt[i]}\t{pr[i]}\t{wt[i]}\t{tat[i]}");
        }
    }
}
using System;

class SRTF
{
    static void Main()
    {
        Console.Write("Enter number of processes: ");
        int n = int.Parse(Console.ReadLine());

        int[] bt = new int[n];
        int[] rt = new int[n];
        int[] wt = new int[n];

        Console.WriteLine("Enter burst times:");
        for (int i = 0; i < n; i++)
        {
            bt[i] = int.Parse(Console.ReadLine());
            rt[i] = bt[i];
        }

        int time = 0, done = 0;

        while (done < n)
        {
            int min = int.MaxValue, idx = -1;
            for (int i = 0; i < n; i++)
                if (rt[i] > 0 && rt[i] < min)
                {
                    min = rt[i];
                    idx = i;
                }

            rt[idx]--;
            time++;

            if (rt[idx] == 0)
            {
                done++;
                wt[idx] = time - bt[idx];
            }
        }

        Console.WriteLine("\nP\tBT\tWT\tTAT");
        for (int i = 0; i < n; i++)
            Console.WriteLine($"P{i + 1}\t{bt[i]}\t{wt[i]}\t{bt[i] + wt[i]}");
    }
}
using System;

class PriorityP
{
    static void Main()
    {
        Console.Write("Enter number of processes: ");
        int n = int.Parse(Console.ReadLine());

        int[] bt = new int[n];
        int[] pr = new int[n];
        int[] rt = new int[n];
        int[] wt = new int[n];

        Console.WriteLine("Enter burst time and priority:");
        for (int i = 0; i < n; i++)
        {
            bt[i] = int.Parse(Console.ReadLine());
            pr[i] = int.Parse(Console.ReadLine());
            rt[i] = bt[i];
        }

        int time = 0, done = 0;

        while (done < n)
        {
            int min = int.MaxValue, idx = -1;
            for (int i = 0; i < n; i++)
                if (rt[i] > 0 && pr[i] < min)
                {
                    min = pr[i];
                    idx = i;
                }

            rt[idx]--;
            time++;

            if (rt[idx] == 0)
            {
                done++;
                wt[idx] = time - bt[idx];
            }
        }

        Console.WriteLine("\nP\tBT\tPR\tWT\tTAT");
        for (int i = 0; i < n; i++)
            Console.WriteLine($"P{i + 1}\t{bt[i]}\t{pr[i]}\t{wt[i]}\t{bt[i] + wt[i]}");
    }
}
using System;

class RoundRobin
{
    static void Main()
    {
        Console.Write("Enter number of processes: ");
        int n = int.Parse(Console.ReadLine());

        int[] bt = new int[n];
        int[] rt = new int[n];
        int[] wt = new int[n];

        Console.WriteLine("Enter burst times:");
        for (int i = 0; i < n; i++)
        {
            bt[i] = int.Parse(Console.ReadLine());
            rt[i] = bt[i];
        }

        Console.Write("Enter time quantum: ");
        int tq = int.Parse(Console.ReadLine());

        int time = 0;
        bool done;

        do
        {
            done = true;
            for (int i = 0; i < n; i++)
            {
                if (rt[i] > 0)
                {
                    done = false;
                    if (rt[i] > tq)
                    {
                        time += tq;
                        rt[i] -= tq;
                    }
                    else
                    {
                        time += rt[i];
                        wt[i] = time - bt[i];
                        rt[i] = 0;
                    }
                }
            }
        } while (!done);

        Console.WriteLine("\nP\tBT\tWT\tTAT");
        for (int i = 0; i < n; i++)
            Console.WriteLine($"P{i + 1}\t{bt[i]}\t{wt[i]}\t{bt[i] + wt[i]}");
    }
}
