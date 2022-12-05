# Zattt
CRUD 
PERPUSTAKAAN

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Data.SqlClient;

namespace PERPUSTAKAAN_BOGOR
{
    public partial class beranda : Form
    {
        public beranda()
        {
            InitializeComponent();
        }
        SqlConnection con = new SqlConnection("Data Source=(localdb)\\MSSQLLocalDB;Initial Catalog=dbperpus;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=False;ApplicationIntent=ReadWrite;MultiSubnetFailover=False");
        SqlCommand cmd;
        SqlDataReader read;
        SqlDataAdapter drr;
        bool clicked = true;
        string sql;

        private void beranda_Load(object sender, EventArgs e)
        {
            display();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            con.Open();
            if (clicked)
            {
                SqlCommand cmd = new SqlCommand($"INSERT INTO buku (judul_buku,penerbit,pengarang,tahun,jenis_buku,lokasi_rak) VALUES ('{textBox2.Text}','{textBox3.Text}','{textBox4.Text}','{textBox5.Text}','{textBox6.Text}','{textBox7.Text}')", con);
                SqlDataAdapter sda = new SqlDataAdapter(cmd);
                cmd.ExecuteNonQuery();
                con.Close();
                MessageBox.Show("data Sudah Dimasukan");
                display();
            }
        }
        public void display()
        {
            SqlCommand cmd = new SqlCommand($"SELECT * FROM buku", con);
            SqlDataAdapter sda = new SqlDataAdapter(cmd);
            DataTable dt = new DataTable();
            sda.Fill(dt);
            dataGridView1.DataSource = dt;
        }

        private void button2_Click(object sender, EventArgs e)
        {
            var idbuku = textBox1.Text;
            var judul_buku = textBox2.Text;
            var penerbit = textBox3.Text;
            var pengarang = textBox4.Text;
            var tahun = textBox5.Text;
            var jenis_buku = textBox6.Text;
            var lokasi_rak = textBox7.Text;
            if (clicked)
            {
                SqlCommand cmd = new SqlCommand($"update buku set judul_buku='{judul_buku}',penerbit='{penerbit}',pengarang='{pengarang}',tahun='{tahun}',jenis_buku='{jenis_buku}',lokasi_rak='{lokasi_rak}' where idbuku='{idbuku}'", con);
                con.Open();
                cmd.ExecuteNonQuery();
                con.Close();
                MessageBox.Show("data sudah terupdate");
                display();
            }
        }

        private void button3_Click(object sender, EventArgs e)
        {
            var idbuku = textBox1.Text;
            var judul_buku = textBox2.Text;
            var penerbit = textBox3.Text;
            var pengarang = textBox4.Text;
            var tahun = textBox5.Text;
            var jenis_buku = textBox6.Text;
            var lokasi_rak = textBox7.Text;
            con.Open();
            if (clicked)
            {
                SqlCommand cmd = new SqlCommand($"delete from buku where idbuku ='{idbuku}'", con);
                cmd.ExecuteNonQuery();
                con.Close();
                MessageBox.Show("data sudah delete");
                display();
            }
        }

        private void dataGridView1_CellContentClick(object sender, DataGridViewCellEventArgs e)
        {

        }
        public void search(string valueSearch)
        {
            SqlDataAdapter sda = new SqlDataAdapter($"select * from buku where concat (idbuku,judul_buku,penerbit,pengarang,tahun,jenis_buku,lokasi_rak ) like '%{valueSearch}%'", con);
            DataTable dt = new DataTable();
            sda.Fill(dt);
            dataGridView1.DataSource = dt;
        }

        private void button4_Click(object sender, EventArgs e)
        {
            string valueSearch = textBox8.Text.ToString();
            search(valueSearch);
        }
    }
}
