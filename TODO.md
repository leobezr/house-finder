## Phase 1 - Setup
- [x] Install Docker
- [ ] Run n8n locally
- [ ] Create a Git repository
- [ ] Create first workflow
- [ ] Connect Gmail (or IMAP)
- [ ] Verify email trigger works

## Phase 2 - Read Idealista Alerts
- [ ] Receive new Idealista email
- [ ] Filter only alert emails
- [ ] Extract listing title
- [ ] Extract listing URL
- [ ] Extract price
- [ ] Extract location
- [ ] Extract area (m²)

## Phase 3 - Store Listings
- [ ] Create Google Sheet
- [ ] Add listing to sheet
- [ ] Prevent duplicate URLs
- [ ] Record date found

## Phase 4 - Scoring
- [ ] Calculate €/m²
- [ ] Create simple scoring function
- [ ] Flag listings above score threshold
- [ ] Sort by highest score

## Phase 5 - Notifications
- [ ] Send Telegram notification
- [ ] Include score
- [ ] Include price
- [ ] Include direct listing URL

## Phase 6 - Improvements
- [ ] Configurable budget
- [ ] Configurable locations
- [ ] Configurable minimum area
- [ ] Detect price reductions
- [ ] Detect reposted listings
- [ ] Weekly summary report