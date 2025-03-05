package pruebaconexionjdbs; 

import java.sql.*;
import java.util.logging.Level;
import java.util.logging.Logger;

public class PruebaConexionJDBS { 

    public static void main(String[] args) {
        String usuario = "root";
        String password = "";
        String url = "jdbc:mysql://localhost:3306/prueba"; 
        
        Connection conexion = null;
        Statement statement = null;
        ResultSet rs = null;
        
        try {
            
            Class.forName("com.mysql.cj.jdbc.Driver");

            
            conexion = DriverManager.getConnection(url, usuario, password);
            statement = conexion.createStatement();

            
            String sqlInsert = "INSERT INTO USUARIO (USERNAME, USERPASSWORD) VALUES ('USUARIO4', 'CLAVE4')";
            statement.executeUpdate(sqlInsert);
            System.out.println("Registro insertado correctamente.");

            
            String sqlSelect = "SELECT * FROM USUARIO";
            rs = statement.executeQuery(sqlSelect);

            System.out.println("Lista de usuarios:");
            while (rs.next()) {
                System.out.println(rs.getInt("userid") + ": " + rs.getString("username"));
            }
            
            String sqlUpdate = "UPDATE USUARIO SET USERPASSWORD = 'NUEVACONTRASEÑA' WHERE USERNAME = 'ABC'";
            int filasActualizadas = statement.executeUpdate(sqlUpdate);
            if (filasActualizadas > 0) {
                System.out.println("Usuario actualizado correctamente.");
            } else {
                System.out.println("No se encontró el usuario.");
            }
            
            String sqlDelete = "DELETE FROM USUARIO WHERE USERNAME = 'usuario1'";
            int filasEliminadas = statement.executeUpdate(sqlDelete);
            if (filasEliminadas > 0) {
                System.out.println("Usuario eliminado correctamente.");
            } else {
                System.out.println("No se encontró el usuario para eliminar.");
            }
            
        } catch (ClassNotFoundException e) {
            System.out.println("Error: No se encontró el driver de MySQL.");
            e.printStackTrace();
        } catch (SQLException e) {
            System.out.println("Error en la conexión o en la consulta SQL.");
            e.printStackTrace();
        } finally {
            
            try {
                if (rs != null) rs.close();
                if (statement != null) statement.close();
                if (conexion != null) conexion.close();
                System.out.println("Conexión cerrada.");
            } catch (SQLException e) {
                System.out.println("Error al cerrar la conexión.");
                e.printStackTrace();
            }
        }
    }
}
