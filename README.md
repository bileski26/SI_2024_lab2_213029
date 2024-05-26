# SI_2024_lab2_213029
Gorjan Bileski 213029

# Control Flow Graph
![Control Flow Graph](https://github.com/bileski26/SI_2024_lab2_213029/assets/149583510/7cd9d6ed-eef3-482a-83fb-56c519cd42f5)

# Цикломатска комплексност
М = E - N + 2P
E - број ребра во графот
N - број темиња во графот
P - број на различни компоненти

Ние имаме 24 темиња, 32 ребра и 1 компонента. М = 32 - 24 + 2, М = 32 - 24 + 2, checkCart = 10

# Тест случаи според критериумот Every Branch 
[Тест случаи според критериумот Every Branch.xlsx](https://github.com/bileski26/SI_2024_lab2_213029/files/15444613/Every.Branch.xlsx)

# Multiple Condition критериум
item.getPrice() > 300
item.getDiscount() > 0
item.getBarcode().charAt(0) == '0'

item.getPrice() <= 300
item.getDiscount() <= 0
item.getBarcode().charAt(0) != '0'

# Unit тестови
Се фрла исклучок каде што фрлениот исклучок го зачувуваме во променлива ex. Потоа проверуваме дали тој исклучок ја содржи истата порака како и во главниот код.
 
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.Assertions;
import java.util.Arrays;
import java.util.List;

public class SILab2Test {

    @Test
    public void Every_Statement() {

        //testNullItems
        List<Item> allItems = null;
        RuntimeException ex;
        ex = Assertions.assertThrows(RuntimeException.class, () -> SILab2.checkCart(allItems, 100));
        Assertions.assertTrue(ex.getMessage().contains("allItems list can't be null!"));

        //testItemsWithoutName
        List<Item> itemsWithNameNull = Arrays.asList(
                new Item(null, "12345", 50, 0.1F),
                new Item("", "67890", 100, 0.0F)
        );
        Assertions.assertDoesNotThrow(() -> SILab2.checkCart(itemsWithNameNull, 100));
        Assertions.assertFalse(() -> SILab2.checkCart(itemsWithNameNull, 100));

        //testNullBarcode
        List<Item> itemsWithBarcodeNull = Arrays.asList(
                new Item("Item1", null, 50, 0.1F),
                new Item(null, "12345", 50, 0.1F)

        );
        ex = Assertions.assertThrows(RuntimeException.class, () -> SILab2.checkCart(itemsWithBarcodeNull, 100));
        Assertions.assertTrue(ex.getMessage().contains("No barcode!"));

        //testInvalidBarcode
        List<Item> itemsWithInvalidBarcode = Arrays.asList(
                new Item("Item1", "123A45", 50, 0.1F),
                new Item("Item2", "67890", 100, 0.0F)
        );
        ex = Assertions.assertThrows(RuntimeException.class, () -> SILab2.checkCart(itemsWithInvalidBarcode, 100));
        Assertions.assertTrue(ex.getMessage().contains("Invalid character in item barcode!"));

        //testSpecialDiscount
        List<Item> itemsForSpecialDiscount = Arrays.asList(
                new Item("Item1", "012345", 400, 0.1F),
                new Item("Item2", "12345", 200, 0.1F)
        );
        Assertions.assertTrue(SILab2.checkCart(itemsForSpecialDiscount, 1000));

    }

    @Test
    public void testSpecialDiscountCondition(){
        List<Item> allItems = Arrays.asList(
                new Item("Product A", "012345", 400, 0.2f),
                new Item("Product B", "123456", 200, 0.1f),
                new Item("Product C", "034567", 500, 0.25f)
        );

        float expectedSum = (float) ((400*0.2F)-30 + (200*0.1F) + (500*0.25)-30);

        Assertions.assertTrue(SILab2.checkCart(allItems,(int)expectedSum));

    }

}


