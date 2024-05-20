# Lab 3: Aplicación de técnicas efectivas de redacción de exámenes

**Scenario 1: User Authentication Tests**

<br>

Issue

* Si se prueba más de un caso de uso en la misma prueba, la prueba debe dividirse en al menos dos.
* El nombre de la prueba no es descriptivo.

```
TEST UserAuthentication
  ASSERT_TRUE(authenticate("validUser", "validPass"), "Should succeed with correct credentials")
  ASSERT_FALSE(authenticate("validUser", "wrongPass"), "Should fail with wrong credentials")
END TEST
```
<br>


Issue 

* El nombre no es descriptivo.
* El objetivo de la prueba no está claro ya que menciona qué datos de proceso pero no qué aspecto de los datos se está validando o procesando.
* Resultar positivo cuando el escenario de excepción en la misma prueba
* Las funciones auxiliares de prueba deben tener nombres claros.

<br>

```
SETUP
  initializeAuthenticationService()
END SETUP

TEARDOWN
  resetAuthenticationService()
END TEARDOWN
TEST AuthenticateWithValidCredentials
  ASSERT_TRUE(authenticate("validUser", "validPass"), "Should succeed with correct credentials")
END TEST

TEST AuthenticateWithInvalidPassword
  ASSERT_FALSE(authenticate("validUser", "wrongPass"), "Should fail with wrong credentials")
END TEST

TEST AuthenticateWithNonExistentUser
  ASSERT_FALSE(authenticate("nonExistentUser", "validPass"), "Should fail with non-existent user")
END TEST

TEST AuthenticateWithEmptyCredentials
  ASSERT_FALSE(authenticate("", ""), "Should fail with empty credentials")
END TEST

TEST AuthenticateWithNullCredentials
  TRY
    authenticate(NULL, NULL)
    ASSERT_FAIL("Should throw an exception with null credentials")
  CATCH Exception e
    ASSERT_TRUE(e is InvalidArgumentException, "Should throw InvalidArgumentException with null credentials")
END TEST
```
<br>
 

**Scenario 2: Data Processing Functions**

<br>

```
TEST DataProcessing
  DATA data = fetchData()
  TRY
    processData(data)
    ASSERT_TRUE(data.processedSuccessfully, "Data should be processed successfully")
  CATCH error
    ASSERT_EQUALS("Data processing error", error.message, "Should handle processing errors")
  END TRY
END TEST
```

<br>

***Revised test case(s):***

<br>

```
SETUP
  initializeDataProcessingEnvironment()
END SETUP

TEARDOWN
  resetDataProcessingEnvironment()
END TEARDOWN

TEST ProcessDataSuccessfully
  DATA data = fetchData()
  TRY
    processData(data)
    ASSERT_TRUE(data.processedSuccessfully, "Data should be processed successfully")
  CATCH error
    ASSERT_FAIL("No error should be thrown during successful data processing")
END TEST

TEST HandleDataProcessingError
  DATA data = fetchInvalidData()  
  TRY
    processData(data)
    ASSERT_FAIL("An error should be thrown during data processing")
  CATCH error
    ASSERT_EQUALS("Data processing error", error.message, "Should handle processing errors")
END TEST

TEST ProcessNullData
  TRY
    processData(NULL)
    ASSERT_FAIL("Should throw an exception when processing null data")
  CATCH error
    ASSERT_TRUE(error is InvalidArgumentException, "Should throw InvalidArgumentException when processing null data")
END TEST

TEST ProcessEmptyData
  DATA data = fetchData("")  // Assuming this fetches empty data
  TRY
    processData(data)
    ASSERT_FALSE(data.processedSuccessfully, "Data should not be processed successfully")
  CATCH error
    ASSERT_EQUALS("Empty data error", error.message, "Should handle empty data processing errors")
END TEST
```

<br>

###### **Scenario 3: UI Responsiveness**

<br>

```
TEST UIResponsiveness
  UI_COMPONENT uiComponent = setupUIComponent(1024)
  ASSERT_TRUE(uiComponent.adjustsToScreenSize(1024), "UI should adjust to width of 1024 pixels")
END TEST
```

<br>

Issues:

* El nombre no es descriptivo.
* El objetivo de la prueba no está claro.
* Las funciones de configuración cuando sea posible deben definirse globalmente para todas las pruebas.

<br>

***Revised test case(s):***

<br>

```
SETUP
  initializeUIFramework()
END SETUP

TEARDOWN
  disposeUIFramework()
END TEARDOWN

TEST AdjustsToStandardScreenSize
  UI_COMPONENT uiComponent = setupUIComponent()
  uiComponent.setScreenSize(1024)
  ASSERT_TRUE(uiComponent.adjustsToScreenSize(1024), "UI should adjust to width of 1024 pixels")
END TEST

TEST AdjustsToSmallScreenSize
  UI_COMPONENT uiComponent = setupUIComponent()
  uiComponent.setScreenSize(480)
  ASSERT_TRUE(uiComponent.adjustsToScreenSize(480), "UI should adjust to width of 480 pixels")
END TEST

TEST AdjustsToLargeScreenSize
  UI_COMPONENT uiComponent = setupUIComponent()
  uiComponent.setScreenSize(1920)
  ASSERT_TRUE(uiComponent.adjustsToScreenSize(1920), "UI should adjust to width of 1920 pixels")
END TEST

TEST AdjustsToZeroScreenSize
  UI_COMPONENT uiComponent = setupUIComponent()
  uiComponent.setScreenSize(0)
  ASSERT_FALSE(uiComponent.adjustsToScreenSize(0), "UI should not adjust to width of 0 pixels")
END TEST

TEST AdjustsToNegativeScreenSize
  UI_COMPONENT uiComponent = setupUIComponent()
  uiComponent.setScreenSize(-100)
  ASSERT_FALSE(uiComponent.adjustsToScreenSize(-100), "UI should not adjust to negative screen size")
END TEST

```