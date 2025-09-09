import java.util.ArrayList;
import java.util.Scanner;

public class SistemaCompra {
    // Lista para almacenar los productos
    private static ArrayList<String> nombresProductos = new ArrayList<>();
    private static ArrayList<Double> preciosProductos = new ArrayList<>();
    private static ArrayList<Integer> cantidadesProductos = new ArrayList<>();
    
    // Scanner para entrada de datos
    private static Scanner scanner = new Scanner(System.in);
    
    public static void main(String[] args) {
        mostrarMenuPrincipal();
    }
    
    /**
     * Método para mostrar el menú principal y manejar las opciones
     */
    public static void mostrarMenuPrincipal() {
        int opcion;
        
        do {
            System.out.println("\n=== SISTEMA DE COMPRA ===");
            System.out.println("1. Agregar producto");
            System.out.println("2. Ver carrito");
            System.out.println("3. Pagar");
            System.out.println("4. Salir");
            System.out.print("Seleccione una opción: ");
            
            opcion = scanner.nextInt();
            scanner.nextLine(); // Limpiar buffer
            
            switch (opcion) {
                case 1:
                    agregarProducto();
                    break;
                case 2:
                    mostrarCarrito();
                    break;
                case 3:
                    procesarPago();
                    break;
                case 4:
                    System.out.println("¡Gracias por usar nuestro sistema!");
                    break;
                default:
                    System.out.println("Opción no válida. Intente nuevamente.");
            }
        } while (opcion != 4);
    }
    
    /**
     * Método para agregar un producto al carrito
     */
    public static void agregarProducto() {
        System.out.println("\n--- AGREGAR PRODUCTO ---");
        
        System.out.print("Ingrese el nombre del producto: ");
        String nombre = scanner.nextLine();
        
        System.out.print("Ingrese el precio del producto: ");
        double precio = scanner.nextDouble();
        
        System.out.print("Ingrese la cantidad: ");
        int cantidad = scanner.nextInt();
        scanner.nextLine(); // Limpiar buffer
        
        // Validar datos de entrada
        if (precio <= 0 || cantidad <= 0) {
            System.out.println("Error: El precio y la cantidad deben ser mayores a 0.");
            return;
        }
        
        // Agregar producto a las listas
        nombresProductos.add(nombre);
        preciosProductos.add(precio);
        cantidadesProductos.add(cantidad);
        
        System.out.println("✅ Producto agregado correctamente!");
    }
    
    /**
     * Método para mostrar el contenido del carrito
     */
    public static void mostrarCarrito() {
        if (nombresProductos.isEmpty()) {
            System.out.println("\nEl carrito está vacío.");
            return;
        }
        
        System.out.println("\n--- CARRITO DE COMPRAS ---");
        double total = 0;
        
        for (int i = 0; i < nombresProductos.size(); i++) {
            double subtotal = preciosProductos.get(i) * cantidadesProductos.get(i);
            total += subtotal;
            
            System.out.printf("%s - $%.2f x %d = $%.2f%n",
                nombresProductos.get(i),
                preciosProductos.get(i),
                cantidadesProductos.get(i),
                subtotal);
        }
        
        System.out.printf("TOTAL: $%.2f%n", total);
    }
    
    /**
     * Método para calcular el total de la compra
     * @return Total de la compra
     */
    public static double calcularTotal() {
        double total = 0;
        
        for (int i = 0; i < nombresProductos.size(); i++) {
            total += preciosProductos.get(i) * cantidadesProductos.get(i);
        }
        
        return total;
    }
    
    /**
     * Método para validar un correo electrónico
     * @param email Correo a validar
     * @return true si es válido, false si no lo es
     */
    public static boolean validarEmail(String email) {
        // Validación simple de email
        return email.contains("@") && email.contains(".") && email.length() > 5;
    }
    
    /**
     * Método para procesar el pago y generar el reporte
     */
    public static void procesarPago() {
        if (nombresProductos.isEmpty()) {
            System.out.println("\nError: No hay productos en el carrito.");
            return;
        }
        
        System.out.println("\n--- PROCESAR PAGO ---");
        
        // Validar email
        String email;
        do {
            System.out.print("Ingrese su correo electrónico: ");
            email = scanner.nextLine();
            
            if (!validarEmail(email)) {
                System.out.println("Correo electrónico no válido. Intente nuevamente.");
            }
        } while (!validarEmail(email));
        
        // Calcular total
        double total = calcularTotal();
        
        // Generar y mostrar reporte
        generarReporte(email, total);
        
        // Limpiar carrito después del pago
        limpiarCarrito();
    }
    
    /**
     * Método para generar el reporte de compra
     * @param email Email del cliente
     * @param total Total de la compra
     */
    public static void generarReporte(String email, double total) {
        System.out.println("\n=== REPORTE DE COMPRA ===");
        System.out.println("Cliente: " + email);
        System.out.println("Fecha: " + java.time.LocalDate.now());
        System.out.println("--- DETALLE DE COMPRA ---");
        
        for (int i = 0; i < nombresProductos.size(); i++) {
            System.out.printf("%d. %s - $%.2f x %d%n",
                i + 1,
                nombresProductos.get(i),
                preciosProductos.get(i),
                cantidadesProductos.get(i));
        }
        
        System.out.println("-------------------------");
        System.out.printf("TOTAL A PAGAR: $%.2f%n", total);
        System.out.println("¡Gracias por su compra!");
        System.out.println("=========================");
    }
    
    /**
     * Método para limpiar el carrito después del pago
     */
    public static void limpiarCarrito() {
        nombresProductos.clear();
        preciosProductos.clear();
        cantidadesProductos.clear();
    }
}
