
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace SultTableServiceApp
{
    public partial class MainForm : Form
    {
        public MainForm()
        { 
            
            InitializeComponent();
 
        }
        //Declare Field Variables

        int TotalPizzas,TotalCompanyTransactions, Margherita, 
            Pepperoni, Pineapple, TotalNoPizzas;
        decimal TotalCost, TotalCompanyReceipts, AvgTransactionValue;
         const decimal MARGHERITA_PRICE = 9.00M, PEPPERONI_PRICE = 11.50M,
            PINEAPPLE_PRICE = 12.79M, SURCHARGE = 2.49M;
       
        //Focus placed on start button when the form loads
        private void MainForm_Load(object sender, EventArgs e)
        {
            Start_Button.Focus();
        }

        /* Event handler that makes Pizza menu, order panel and Sult logo
         * visible. Main Form shows server name and table number
         */
        private void Start_Button_Click(object sender, EventArgs e)
        {
            OrderSummaryPanel.Visible = true;
            MenuGroupBox.Visible = true;
            StartPanel.Visible = false;
            SultLogoPictureBox.Visible = true;
            this.Text = ServerNameTextBox.Text + " @ Table Number " 
           + TableNoTextBox.Text; // Main form displays this text
            Summary_Button.Enabled = false;
        }
        //Event handler that displays order summary. Relevant buttons become visible and enabled
        private void Order_Button_Click(object sender, EventArgs e)
        {
            TableOrderGroupBox.Visible = true;
            StartPanel.Visible = false;
            Clear_Button.Focus();
            Order_Button.Enabled = false;
            MenuGroupBox.Enabled = false;
            Summary_Button.Enabled = true;
           
            //Tooltips to guide user 
            ClearDataToolTip.SetToolTip(Clear_Button, "Press this to reset for next user");
            ExitToolTip.SetToolTip(ExitButton, "Press this to exit application");
           
            //Shows servername information in the order summary
              ServerLabel.Text = ServerNameTextBox.Text;
           
            //try catches to prevent against input error for pizzas
            try
            {
                Margherita = int.Parse(MargheritaTextBox.Text);
            }
            catch
            {
                MessageBox.Show("Please enter a whole number of margherita pizzas",
               "Input Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
                MenuGroupBox.Enabled = true; Order_Button.Enabled = true;
                MargheritaTextBox.Focus(); MargheritaTextBox.SelectAll();

            }
                try
                {
                    Pepperoni = int.Parse(PepperoniTextBox.Text);
                }
                catch
                {
                    MessageBox.Show("Please enter a whole number of pepperoni pizzas", "Input Error",
                    MessageBoxButtons.OK, MessageBoxIcon.Error);
                    MenuGroupBox.Enabled = true; Order_Button.Enabled = true;
                    PepperoniTextBox.Focus(); PepperoniTextBox.SelectAll();

                }
            
                try
                {
                    Pineapple = int.Parse(PineappleTextBox.Text);
               
                //Add up number of pizzas
                TotalPizzas = Margherita + Pepperoni + Pineapple;
                //Outputs total pizzas to TotalPizzasLabel
                TotalPizzaLabel.Text = TotalPizzas.ToString();

                //Declare local variables for pizza price
                decimal MargheritaTotalPrice, PepperoniTotalPrice, PineappleTotalPrice;
                
                //Multiply price by quantity
                MargheritaTotalPrice = Margherita * MARGHERITA_PRICE;
                PepperoniTotalPrice = Pepperoni * PEPPERONI_PRICE;
                PineappleTotalPrice = Pineapple * PINEAPPLE_PRICE;
                
                 //Calculate cost of pizzas and add Surcharge
                TotalCost = MargheritaTotalPrice + PepperoniTotalPrice + PineappleTotalPrice
                    + SURCHARGE;
                //Output cost to receipts label
                ReceiptsLabel.Text = TotalCost.ToString();

                // Calculations for company summary
                TotalCompanyTransactions ++; //Increase number of transactions by 1
                TotalNoPizzas += TotalPizzas;
                TotalCompanyReceipts += TotalCost;
                
                // Output to company summary
             TotalNoPizzasLabel.Text = TotalNoPizzas.ToString();
             TotalCompanyReceipts_Label.Text = TotalCompanyReceipts.ToString("c2");
                Total_TransactionsLabel.Text = TotalCompanyTransactions.ToString();
            }
                catch
                {
                    MessageBox.Show("Please enter a whole number of Ham Pineapple pizzas", "Input Error",
                    MessageBoxButtons.OK, MessageBoxIcon.Error);
                    MenuGroupBox.Enabled = true; Order_Button.Enabled = true;
                    PineappleTextBox.Focus(); PineappleTextBox.SelectAll();

                }
             
        }
        //Event handler for the company summary. Relevant buttons become visible and enabled
        private void Summary_Button_Click(object sender, EventArgs e)
        {   
            StartPanel.Visible = false;
            Order_Button.Enabled = false;
            Summary_Button.Enabled = true;
            CompanySummaryGroupBox.Visible = true;
            Clear_Button.Focus();
            this.Text = "Sult Company Summary"; // Main form displays this text
          
            //Calaculate avergae transaction value
            AvgTransactionValue = TotalCompanyReceipts /TotalNoPizzas ;
         // Output the value to the average transaction label
            AvegTransactionLabel.Text = AvgTransactionValue.ToString("c2");
        }

          //Event handler that resets the form for the next user
        private void Clear_Button_Click(object sender, EventArgs e)
        {
            ServerNameTextBox.Focus();
            ServerLabel.Text= "";
            ServerNameTextBox.Clear();
            MargheritaTextBox.Text=  "0";
            PepperoniTextBox.Text="0" ;
            PineappleTextBox.Text= "0" ;
            TotalPizzaLabel.Text = "";
            ReceiptsLabel.Text = "";
            TableNoTextBox.Clear();
            this.Text = "Welcome to Sult Bar and Restaurant";
            MenuGroupBox.Enabled = true;
            Order_Button.Enabled = true;
            StartPanel.Visible = true;
            MenuGroupBox.Visible = false;
            OrderSummaryPanel.Visible = false;
            TableOrderGroupBox.Visible = false;
            SultLogoPictureBox.Visible = false;
        }

        //Event handler that closes the form
        private void ExitButton_Click(object sender, EventArgs e)
        {
            this.Close();
        }
    }
}
