# archthegit
###### /java/guitests/guihandles/DetailsPanelHandle.java
``` java

/**
 * A handle to the {@code DetailsPanel} of the application.
 */
public class DetailsPanelHandle extends NodeHandle<Node> {
    public static final String DETAILS_PANEL_ID = "#detailsPanelPlaceholder";

    private static final String NAME_FIELD_ID = "#name";
    private static final String ADDRESS_FIELD_ID = "#address";
    private static final String PHONE_FIELD_ID = "#phone";
    private static final String EMAIL_FIELD_ID = "#email";
    private static final String HOME_PHONE_FIELD_ID = "#homePhone";
    private static final String SCHOOL_EMAIL_FIELD_ID = "#schoolEmail";
    private static final String BIRTHDAY_FIELD_ID = "#birthday";
    private static final String WEBSITE_FIELD_ID = "#website";
    private static final String TAGS_FIELD_ID = "#tags";
    private static final String ADDRESS_FIELD_FIELD_ID = "#addressField";
    private static final String PHONE_FIELD_FIELD_ID = "#phoneField";
    private static final String HOME_PHONE_FIELD_FIELD_ID = "#homePhoneField";
    private static final String EMAIL_FIELD_FIELD_ID = "#emailField";
    private static final String SCHOOL_EMAIL_FIELD_FIELD_ID = "#schoolEmailField";
    private static final String BIRTHDAY_FIELD_FIELD_ID = "#birthdayField";
    private static final String WEBSITE_FIELD_FIELD_ID = "#websiteField";

    private final Label nameLabel;
    private final Label addressLabel;
    private final Label phoneLabel;
    private final Label homePhoneLabel;
    private final Label emailLabel;
    private final Label schEmailLabel;
    private final Label birthdayLabel;
    private final Label websiteLabel;
    private final List<Label> tagLabels;

    private final Text addressText;
    private final Text phoneText;
    private final Text homePhoneText;
    private final Text emailText;
    private final Text schEmailText;
    private final Text birthdayText;
    private final Text websiteText;

    private String latestName;
    private String latestAddress;
    private String latestPhone;
    private String latestEmail;
    private String latestSchoolEmail;
    private String latestBirthday;
    private String latestWebsite;
    private String latestHomePhone;
    private List<String> latestTags;

    public DetailsPanelHandle(Node detailsPanelNode) {
        super(detailsPanelNode);

        this.nameLabel = getChildNode(NAME_FIELD_ID);
        this.addressLabel = getChildNode(ADDRESS_FIELD_ID);
        this.phoneLabel = getChildNode(PHONE_FIELD_ID);
        this.emailLabel = getChildNode(EMAIL_FIELD_ID);
        this.schEmailLabel = getChildNode(SCHOOL_EMAIL_FIELD_ID);
        this.birthdayLabel = getChildNode(BIRTHDAY_FIELD_ID);
        this.websiteLabel = getChildNode(WEBSITE_FIELD_ID);
        this.homePhoneLabel = getChildNode(HOME_PHONE_FIELD_ID);

        Region tagsContainer = getChildNode(TAGS_FIELD_ID);
        this.tagLabels = tagsContainer
                .getChildrenUnmodifiable()
                .stream()
                .map(Label.class::cast)
                .collect(Collectors.toList());

        this.addressText = getChildNode(ADDRESS_FIELD_FIELD_ID);
        this.phoneText = getChildNode(PHONE_FIELD_FIELD_ID);
        this.emailText = getChildNode(EMAIL_FIELD_FIELD_ID);
        this.schEmailText = getChildNode(SCHOOL_EMAIL_FIELD_FIELD_ID);
        this.birthdayText = getChildNode(BIRTHDAY_FIELD_FIELD_ID);
        this.websiteText = getChildNode(WEBSITE_FIELD_FIELD_ID);
        this.homePhoneText = getChildNode(HOME_PHONE_FIELD_FIELD_ID);
    }

    public String getName() {
        return nameLabel.getText();
    }

    public String getAddress() {
        return addressLabel.getText();
    }

    public String getPhone() {
        return phoneLabel.getText();
    }

    public String getEmail() {
        return emailLabel.getText();
    }

    public String getSchEmail() {
        return schEmailLabel.getText();
    }

    public String getBirthday() {
        return birthdayLabel.getText();
    }

    public String getWebsite() {
        return  websiteLabel.getText();
    }

    public String getHomePhone() {
        return homePhoneLabel.getText();
    }

    public String getAddressField() {
        return addressText.getText();
    }

    public String getPhoneField() {
        return phoneText.getText();
    }

    public String getHomePhoneField() {
        return homePhoneText.getText();
    }

    public String getEmailField() {
        return emailText.getText();
    }

    public String getSchEmailField() {
        return schEmailText.getText();
    }

    public String getBirthdayField() {
        return birthdayText.getText();
    }

    public String getWebsiteField () {
        return websiteText.getText();
    }

    public List<String> getTags() {
        return tagLabels
                .stream()
                .map(Label::getText)
                .collect(Collectors.toList());
    }

    /**
     * Remember the details of the person that was last selected
     */
    public void rememberSelectedPersonDetails() {
        latestAddress = getAddress();
        latestSchoolEmail = getSchEmail();
        latestEmail = getEmail();
        latestName = getName();
        latestPhone = getPhone();
        latestBirthday = getBirthday();
        latestTags = getTags();
        latestWebsite = getWebsite();
        latestHomePhone = getHomePhone();
    }

    /**
     * Returns true if the selected {@code Person} is different from the value remembered by the most recent
     * {@code rememberSelectedPersonDetails()} call.
     */
    public boolean isSelectedPersonChanged() {
        return !getName().equals(latestName)
                || !getAddress().equals(latestAddress)
                || !getPhone().equals(latestPhone)
                || !getSchEmail().equals(latestSchoolEmail)
                || !getEmail().equals(latestEmail)
                || !getBirthday().equals(latestBirthday)
                || !getWebsite().equals(latestWebsite)
                || !getHomePhone().equals(latestHomePhone)
                || !getTags().equals(latestTags);
    }

}

```
###### /java/seedu/address/model/person/BirthdayTest.java
``` java

public class BirthdayTest {
    @Test
    public void isValidBirthday() {
        // invalid phone numbers
        assertFalse(Birthday.isValidBirthday("")); // empty string
        assertFalse(Birthday.isValidBirthday(" ")); // spaces only
        assertFalse(Birthday.isValidBirthday("birthday")); // non-numeric
        assertFalse(Birthday.isValidBirthday("9011p041")); // alphabets within digits
        assertFalse(Birthday.isValidBirthday("2001/2/11")); // not dd/mm/yyyy format

        // valid birthdays
        assertTrue(Birthday.isValidBirthday("11/12/1998"));
        assertTrue(Birthday.isValidBirthday("12/01/1971"));
        assertTrue(Birthday.isValidBirthday("14/01/1986"));

        // birthday is not set
        assertTrue(Birthday.isValidBirthday("NIL"));
        assertTrue(Birthday.isValidBirthday(Birthday.BIRTHDAY_TEMPORARY));
    }

    @Test
    public void testSymmetricHashCode() throws IllegalValueException {
        // equals and hashCode check birthday field value
        Birthday birthdayX = new Birthday("14/01/1986");
        Birthday birthdayY = new Birthday("14/01/1986");
        assertTrue(birthdayX.equals(birthdayY) && birthdayY.equals(birthdayX));
        assertTrue(birthdayX.hashCode() == birthdayY.hashCode());
    }
}



```
###### /java/seedu/address/ui/DetailsPanelTest.java
``` java

    @Test
    public void display() throws Exception {

        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getName());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getAddress());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getAddressField());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getEmail());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getEmailField());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getSchEmail());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getSchEmailField());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getBirthday());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getBirthdayField());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getPhone());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getPhoneField());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getHomePhone());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getHomePhoneField());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getWebsite());
        assertEquals(MESSAGE_EMPTY_STRING, detailsPanelHandle.getWebsiteField());
        assertEquals(new ArrayList<>(), detailsPanelHandle.getTags());
        detailsPanelHandle.rememberSelectedPersonDetails();

        // associated details of a person
        postNow(selectionChangedEventStub);
        assertDetailsDisplay(detailsPanel, ALICE);
        assertTrue(detailsPanelHandle.isSelectedPersonChanged());
        detailsPanelHandle.rememberSelectedPersonDetails();

        // asserts that no change is registered when same person is clicked
        postNow(selectionChangedEventStub);
        assertDetailsDisplay(detailsPanel, ALICE);
        assertFalse(detailsPanelHandle.isSelectedPersonChanged());

        // associated info of next person
        selectionChangedEventStub = new PersonPanelSelectionChangedEvent(new PersonCard(BOB, 1));
        postNow(selectionChangedEventStub);
        assertTrue(detailsPanelHandle.isSelectedPersonChanged());
        assertDetailsDisplay(detailsPanel, BOB);
    }

    @Test
    public void equals() {
        detailsPanel = new DetailsPanel();

        assertTrue(detailsPanel.equals(detailsPanel));

        assertFalse(detailsPanel.equals(detailsPanelHandle));

        DetailsPanel expectedDetailsPanel = new DetailsPanel();

        assertTrue(detailsPanel.equals(expectedDetailsPanel));

        detailsPanel.loadPersonInfo(AMY);
        assertFalse(detailsPanel.equals(expectedDetailsPanel));

        expectedDetailsPanel.loadPersonInfo(AMY);
        assertTrue(detailsPanel.equals(expectedDetailsPanel));
    }

```
###### /java/seedu/address/ui/testutil/GuiTestAssert.java
``` java
    /**
     * Asserts that {@code actualInfo} displays the details of {@code expectedPerson}.
     */
    public static void assertDetailsDisplaysPerson(ReadOnlyPerson expectedPerson, DetailsPanelHandle actualInfo) {
        assertEquals(expectedPerson.getName().fullName, actualInfo.getName());
        assertEquals(expectedPerson.getPhone().value, actualInfo.getPhone());
        assertEquals(expectedPerson.getEmail().value, actualInfo.getEmail());
        assertEquals(expectedPerson.getAddress().value, actualInfo.getAddress());
        assertEquals(expectedPerson.getBirthday().value, actualInfo.getBirthday());
        assertEquals(expectedPerson.getWebsite().value, actualInfo.getWebsite());
        assertEquals(expectedPerson.getSchEmail().value, actualInfo.getSchEmail());
        assertEquals(expectedPerson.getTags().stream().map(tag -> tag.tagName).collect(Collectors.toList()),
                actualInfo.getTags());
    }

    /**
     * Asserts that the list in {@code personListPanelHandle} displays the details of {@code persons} correctly and
     * in the correct order.
     */
    public static void assertListMatching(PersonListPanelHandle personListPanelHandle, ReadOnlyPerson... persons) {
        for (int i = 0; i < persons.length; i++) {
            assertCardDisplaysPerson(persons[i], personListPanelHandle.getPersonCardHandle(i));
        }
    }

    /**
     * Asserts that the list in {@code personListPanelHandle} displays the details of {@code persons} correctly and
     * in the correct order.
     */
    public static void assertListMatching(PersonListPanelHandle personListPanelHandle, List<ReadOnlyPerson> persons) {
        assertListMatching(personListPanelHandle, persons.toArray(new ReadOnlyPerson[0]));
    }

    /**
     * Asserts the size of the list in {@code personListPanelHandle} equals to {@code size}.
     */
    public static void assertListSize(PersonListPanelHandle personListPanelHandle, int size) {
        int numberOfPeople = personListPanelHandle.getListSize();
        assertEquals(size, numberOfPeople);
    }

    /**
     * Asserts the message shown in {@code resultDisplayHandle} equals to {@code expected}.
     */
    public static void assertResultMessage(ResultDisplayHandle resultDisplayHandle, String expected) {
        assertEquals(expected, resultDisplayHandle.getText());
    }
}
```