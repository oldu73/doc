# Mock

***

Mockito in a springboot context.

Test that a filter works as expected.

[Project source on gihub](https://github.com/oldu73/springboot001)

***

Extract from CustomerService.java:

```java
public String onlyCustomersWithPaulAsFirstNameThenLastNameToUpperCase() {
    List<Customer> customers = customerRepository.findAll();

    List<Customer> onlyPauls = customers.stream().filter(customer -> "Paul".equals(customer.getFirstName())).collect(Collectors.toList());

    converter.lastNameToUpperCase(onlyPauls);

    return !onlyPauls.isEmpty() ? String.join(", ", onlyPauls.toString()) : "none";
}
```

Extract from CustomerServiceTest.java:

```java
@InjectMocks
private CustomerService customerService;

@Mock
private CustomerRepository customerRepository;

@Mock
Converter converter;

@Test
public void onlyCustomersWithPaulAsFirstNameThenLastNameToUpperCaseTest_WhenNotOnlyPaulsIn_ThenOnlyPaulsOut() {
    when(customerRepository.findAll()).thenReturn(CUSTOMERS);

    customerService.onlyCustomersWithPaulAsFirstNameThenLastNameToUpperCase();

    @SuppressWarnings("unchecked")
    ArgumentCaptor<List<Customer>> customersCaptor = ArgumentCaptor.forClass(List.class);

    verify(converter, times(1)).lastNameToUpperCase(customersCaptor.capture()); // For not captured/tested argument(s) replace with "any()"

    List<Customer> paulsOut = customersCaptor.getValue();

    assertEquals(4, paulsOut.size());
    assertTrue(paulsOut.stream().allMatch(customer -> PAUL.equals(customer.getFirstName())));
}
```

***
