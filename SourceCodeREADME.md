# GamerConnect004-Repository Login page Code.
Source Code of our GamersConnect Project.
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Data.SqlClient;
using System.Data;

namespace SportsPage
{
    public partial class WebForm1 : System.Web.UI.Page
    {
        SqlCommand cmd = new SqlCommand();
        SqlConnection con = new SqlConnection();
        SqlDataAdapter sda = new SqlDataAdapter();
        DataSet ds = new DataSet();

        protected void Page_Load(object sender, EventArgs e)
        {
            if(Session["user"] != null)
            {
                Response.Redirect("third.aspx");
            }
            else

            con.ConnectionString = "Data Source=bhumikhamar\\sqlexpress;Initial Catalog=sports;Integrated Security=True";
            con.Open();
        }

        protected void Button1_Click(object sender, EventArgs e)
        {
            string user = TextBox1.Text.Trim();
            cmd.CommandText = "select * from sport_regi where email = '" + TextBox1.Text.ToString() + "'and pass = '" + TextBox2.Text.ToString() + "'";
            cmd.Connection = con;
            sda.SelectCommand = cmd;
            sda.Fill(ds, "sport_regi");
            if (ds.Tables[0].Rows.Count > 0)
            {
                Session["user"] = user;
                Response.Redirect("third.aspx");
            }
            string log = "select count(*) from sport_regi where email = '"+ TextBox1.Text.ToString() + "'and pass = '"+ TextBox2.Text.ToString() + "'";
            SqlCommand comd = new SqlCommand(log, con);
            //con.Open();
            int temp = Convert.ToInt32(comd.ExecuteScalar().ToString());
            con.Close();
            if(temp == 1)
            {
                Response.Redirect("third.aspx");
            }
            else
            {
                Label1.ForeColor = System.Drawing.Color.Red;
                Label1.Text = "Your Email Id or Password is Invalid.";
            }
            //session[email] = TextBox1.text;
        }
    }
}

# GamerConnect004-Repository Registration page Code.

using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Data.SqlClient;
using System.Data;

namespace SportsPage
{
    public partial class Secondpage : System.Web.UI.Page
    {
        public Secondpage()
        {

        }
        public string cnstring = "Data Source=bhumikhamar\\sqlexpress;Initial Catalog=sports;Integrated Security=True";

        protected void Page_Load(object sender, EventArgs e)
        {

        }
        protected void Button1_Click(object sender, EventArgs e)
        {
            SqlConnection con = new SqlConnection(cnstring);
            con.Open();

            if (con.State == System.Data.ConnectionState.Open)
            {
                string a = "insert into sport_regi(firstname,lastname,email,pass,Age,Gender,Favourite_game_genre)values('" +txt_fn.Text.ToString()+ "','" +txt_ln.Text.ToString()+ "','" +txt_email.Text.ToString()+ "','" + txt_pass.Text.ToString()+ "','" +DropDownList1.SelectedItem.ToString()+ "','" + RadioButtonList1.Text + "','" +DropDownList4.SelectedItem.ToString()+"')";

                SqlCommand cmd = new SqlCommand(a,con);
                cmd.ExecuteNonQuery();
                Response.Redirect("third.aspx");
                //ClientScript.RegisterClientScriptBlock(this.GetType(), "Connection Success", ClientScript, true);
                //Response.Write("Connection Success");
            }
        }
    }
}


# GamerConnect004-Repository Main page Code.

using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Data;
using System.Data.SqlClient;

namespace SportsPage
{
    public partial class third : System.Web.UI.Page
    {
        SqlCommand cmd = new SqlCommand();
        SqlConnection con = new SqlConnection();
        SqlDataAdapter sda = new SqlDataAdapter();
        DataSet ds = new DataSet();

        protected void Page_Load(object sender, EventArgs e)
        {
            if (Session["user"] == null)
            {
                Response.Redirect("third.aspx");
            }
            else
                con.ConnectionString = "Data Source=bhumikhamar\\sqlexpress;Initial Catalog=sports;Integrated Security=True";
            con.Open();

            showdata();

            Image2.ImageUrl = "Images/gamer.jpg";
        }

        protected void Button1_Click(object sender, EventArgs e)
        {

        }
       public void showdata()
        {
            cmd.CommandText = "select * from sport_regi where email = '" +Session["user"]+"'";
            cmd.Connection = con;
            sda.SelectCommand = cmd;
            sda.Fill(ds);
            Label2.Text = "Welcome,";
            Label1.Text = ds.Tables[0].Rows[0]["firstname"].ToString() +","+ ds.Tables[0].Rows[0]["lastname"].ToString();
           // Image1.ImageUrl = "Images/gamer.jpg";
        }
    }
}



# GamerConnect004-Repository User Profile page Code.
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Data.SqlClient;
using System.Data;

namespace SportsPage
{
    public partial class myprofile : System.Web.UI.Page
    {
        public string ConnectionString = "Data Source=bhumikhamar\\sqlexpress;Initial Catalog=sports;Integrated Security=True";

        protected void Page_Load(object sender, EventArgs e)
        {
            Label4.Text = "Welcome to your Profile";
            Label8.Text = "You Can Update your profile information below.";
        }

        protected void Update_Click(object sender, EventArgs e)
        {
            Label5.Text = upd_fn.Text.ToString();

            SqlConnection con = new SqlConnection(ConnectionString);
            con.Open();

            if (con.State == System.Data.ConnectionState.Open)
            {
                //string up = "UPDATE sport_regi(lastname, firstName, email, pass, Age, Gender, Favourite_game_genre) VALUES ('"+ upd_ln.Text.ToString() + "','" + upd_email.Text.ToString() + "','" + upd_pass.Text.ToString() + "','" + DropDownList3.SelectedItem.ToString() + "','" + RadioButtonList1.Text + "' , '" + DropDownList4.SelectedItem.ToString() + "') WHERE firstname='" + upd_fn.Text.ToString() + "'";
                string up = "update sport_regi set lastname='" + upd_ln.Text.ToString() + "', email='" + upd_email.Text.ToString() + "', pass= '" + upd_pass.Text.ToString() + "', Age= '" + DropDownList3.SelectedItem.ToString() + "', Gender ='" + RadioButtonList1.Text + "',Favourite_game_genre='" + DropDownList4.SelectedItem.ToString() + "' where firstname='" + upd_fn.Text.ToString() + "'";

                SqlCommand cmd = new SqlCommand(up, con);
                cmd.ExecuteNonQuery();
                Response.Write("Updated Success");
                
                /*SqlConnection ab = new SqlConnection(ConnectionString);
                string up = "update sport_regi set lastname'"+upd_ln.Text.ToString()+"', email'"+upd_pass.Text.ToString()+"', pass'"+upd_pass.Text.ToString()+"', Age'"+DropDownList3.ToString()+"', Gender'"+RadioButtonList1.Text+ "',Favourite_game_genre='"+DropDownList4.SelectedItem.ToString()+"' where firstname='"+upd_fn.Text.ToString()+"'";
                SqlCommand cmd = new SqlCommand(up, ab);*/
            }
        }
    }
}
