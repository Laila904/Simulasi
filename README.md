Laporan SimulasiUts
1. Langkah pertama yaitu membuat class Utils untuk mengkonversi data dari ResultSet menjadi model tabel (TableModel)
yang dapat ditampilkan pada komponen tabel di Java Swing.
![Util](https://github.com/user-attachments/assets/613b1148-3e22-4c0f-a8cf-f3c07b82442c)

3. Langkah kedua yaitu  membuat tampilan GUI yang sudah dihubungkan dengan database posgreSQL
![tabel](https://github.com/user-attachments/assets/5e8968cd-193f-4930-b46a-d0475e15cf76)

5.  Langkah ketiga yaitu memanggil dari button yang pertama yaitu insert.
   Tampilan source code dari Insert beserta Tampilan GUI :
   
   private void btnInsertActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
    
        if (txtISBN.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtJudul.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtTahun.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtPenerbit.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else {
            try {
                Class.forName(driver);
                conn = DriverManager.getConnection(koneksi, user, password);
                conn.setAutoCommit(false);

                String sql = "INSERT INTO buku(ISBN, Judul_Buku, Tahun_Terbit, Penerbit) VALUES(?,?,?,?)";
                pstmt = conn.prepareStatement(sql);

                String ISBN, Judul_Buku, Tahun_Terbit, Penerbit;
                ISBN = txtISBN.getText();
                Judul_Buku = txtJudul.getText();
                Tahun_Terbit = txtTahun.getText();
                Penerbit = txtPenerbit.getText();
                pstmt.setString(1, ISBN);
                pstmt.setString(2, Judul_Buku);
                pstmt.setInt(3, Integer.parseInt(Tahun_Terbit));
                pstmt.setString(4, Penerbit);

                pstmt.executeUpdate();
                conn.commit();

                pstmt.close();
                conn.close();

                JOptionPane.showMessageDialog(null, "Succes to insert");
            } catch (ClassNotFoundException | SQLException ex) {
                JOptionPane.showMessageDialog(null, "Failed to insert " + ex.getMessage());
            }
        }
        bersih();

        showTable(); }
                              
    Tampilan ketika diinsert(atau "Tambah")
    ![Insert](https://github.com/user-attachments/assets/70fd91d8-062f-4104-8b19-cb06c3b08f17)
Hasil ketika insert berhasil
![Hasil insert](https://github.com/user-attachments/assets/87844b32-edf1-480f-b563-e6e87746b2d6)

   4. Langkah ketiga yaitu memanggil dari button yang kedua yaitu Update.
   Tampilan source code dari Update beserta Tampilan GUI :

    private void btnUpdateActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
        String ISBN, Judul_Buku, Tahun_Terbit, Penerbit;
        if (txtISBN.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtJudul.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtTahun.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else if (txtPenerbit.getText().equals("")) {
            JOptionPane.showMessageDialog(null, "Data harus terisi");
        } else {
            try {
                Class.forName(driver);
                String sql = "UPDATE buku SET Judul_Buku = ?, Tahun_Terbit = ?, Penerbit = ? WHERE ISBN = ?";
                conn = DriverManager.getConnection(koneksi, user, password);
                pstmt = conn.prepareStatement(sql);

                ISBN = txtISBN.getText();
                Judul_Buku = txtJudul.getText();
                Tahun_Terbit = txtTahun.getText();
                Penerbit = txtPenerbit.getText();

                pstmt.setString(1, Judul_Buku); 
                pstmt.setString(2, Tahun_Terbit); 
                pstmt.setString(3, Penerbit);
                pstmt.setString(4, ISBN);

                int rowsAffected = pstmt.executeUpdate();
                if (rowsAffected > 0) {
                    JOptionPane.showMessageDialog(null, "update Sukses");
                    bersih();
                } else {
                    JOptionPane.showMessageDialog(null, "Update gagal");
                }
            } catch (ClassNotFoundException | SQLException ex) {
                JOptionPane.showMessageDialog(null, "Error: " + ex.getMessage());
            } catch (NumberFormatException ex) {
                JOptionPane.showMessageDialog(null, "ISBN harus berupa angka");
            } finally {
                try {
                    if (pstmt != null) {
                        pstmt.close();
                    }
                    if (conn != null) {
                        conn.close();
                    }
                } catch (SQLException e) {
                    JOptionPane.showMessageDialog(null, "Error closing resources: " + e.getMessage());
                }
            }
        }
        showTable(); }                           
     Tampilan ketika di Update(atau "Ubah")
     ![Update](https://github.com/user-attachments/assets/d92beb0e-367e-403c-9ef8-a1a380ad1aff)
Hasil ketika Update berhasil
![Hasil Update](https://github.com/user-attachments/assets/816fc6a8-2cd2-4855-a969-9aa9121332eb)

 5. Langkah kelia yaitu memanggil dari button yang ketiga yaitu Delete.
   Tampilan source code dari Delete beserta Tampilan GUI :

     private void btnDeleteActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:                             
        String ISBN;
        ISBN = txtISBN.getText();

        try {
            Class.forName(driver);
            conn = DriverManager.getConnection(koneksi, user, password);

            int jawab = JOptionPane.showConfirmDialog(null, "Silakan Konfirmasi?");
            switch (jawab) {
                case JOptionPane.YES_OPTION:
                    String deleteSql = "DELETE FROM buku WHERE ISBN = ?";
                    pstmt = conn.prepareStatement(deleteSql);
                    pstmt.setString(1, ISBN);
                    pstmt.executeUpdate();
                    pstmt.close();
                    conn.close();

                    break;
                case JOptionPane.NO_OPTION:
                    JOptionPane.showMessageDialog(this, "Kamu menjawab tidak");
                    break;
            }
        } catch (ClassNotFoundException | SQLException ex) {
            JOptionPane.showConfirmDialog(null, "Cek Lagi!!!", "Error", JOptionPane.ERROR_MESSAGE);
            ex.printStackTrace();
        } finally {
            if (pstmt != null) {
                try {
                    pstmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        bersih();
        showTable();                 
    }

    Tampilan ketika di Delete(atau "Hapus")
    ![DELETE](https://github.com/user-attachments/assets/1e5a842d-2821-458c-8ae3-c76a944ec674)

    Hasil ketika Delete berhasil
    ![hasil hapus](https://github.com/user-attachments/assets/82c183fb-8f7d-4267-b9ab-eb9c80e6ee95)

 6. langkah keenam yaitu membuat source code showTable yang berfungsi untuk bertanggung jawab untuk menampilkan
    data dari tabel pesanan dalam bentuk GUI (Graphical User Interface).

    public void showTable() {
        try {
            // TODO code application logi
            Class.forName(driver);
            String sql = "SELECT * FROM Buku order by ISBN";
            conn = DriverManager.getConnection(koneksi, user, password);
            stmt = conn.createStatement();

            while (!conn.isClosed()) {

                ResultSet rs = stmt.executeQuery(sql);
                this.tblBuku.setModel(DbUtils.resultSetToTableModel(rs));
                while (rs.next()) {
                    System.out.println(String.valueOf(rs.getObject(1)) + " " + String.valueOf(rs.getObject(2)) + " " + String.valueOf(rs.getObject(3) +
                    " " + String.valueOf(rs.getObject(4))));
                }
                conn.close();//move this outside the while loop
            }

            stmt.close();

        } catch (ClassNotFoundException | SQLException ex) { 
        }}

    
       Tampilan data pada GUI
        ![data](https://github.com/user-attachments/assets/881ce7a3-d6af-4a89-9f2a-6f5aedd9f9d3)

