
1. A query to return the total number of auctions that have a duration of less than 10 days:
sql
Copy code
SELECT COUNT(*) AS "Auctions_Less_Than_10_Days" 
FROM mb_auction 
WHERE (enddate - startdate) < INTERVAL '10 DAYS';
2. For all the auctions that are in progress, show the item description, the date that the auction ends, and the reserve price, ordered by reserve price with the highest first:
sql
Copy code
SELECT mi.description AS "Item Description", 
       ma.enddate AS "End Date", 
       ma.reserve AS "Reserve Price" 
FROM mb_auction ma 
JOIN mb_item mi ON ma.itemid = mi.itemid 
WHERE ma.status = 'in_progress' 
ORDER BY ma.reserve DESC;
3. Display the names of all users who have bid in auctions they have started:
sql
Copy code
SELECT DISTINCT mu.name AS "User Name"
FROM mb_user mu
JOIN mb_auction ma ON mu.userid = ma.userid
JOIN mb_bid mb ON ma.auctionid = mb.auctionid
WHERE mb.bidder = ma.userid;
4. Display the names of all users who have auctioned the same item more than once. This will occur if an auction did not meet its reserve price and the seller decided to auction it again:
sql
Copy code
SELECT mu.name AS "User Name", 
       ma.itemid AS "Item ID", 
       COUNT(ma.itemid) AS "Auction Count"
FROM mb_user mu
JOIN mb_auction ma ON mu.userid = ma.userid
WHERE ma.status = 'reserve_not_met'
GROUP BY mu.name, ma.itemid
HAVING COUNT(ma.itemid) > 1;
5. Display the auction ID and amount of the winning bid for all ‘sold’ auctions:
sql
Copy code
SELECT mb.auctionid AS "Auction ID", 
       MAX(mb.amount) AS "Winning Amount"
FROM mb_bid mb
JOIN mb_auction ma ON mb.auctionid = ma.auctionid
WHERE ma.status = 'sold'
GROUP BY mb.auctionid
ORDER BY "Winning Amount" DESC;
6. List all auctions (pending, sold, or unsold), displaying the end date and the description of the item. For auctions that have a winner, display the name of the winner and the winning bid amount. For auctions that have no winner, display some marker characters (e.g. ‘==’) to show that the winner is missing, and display ‘0’ for the winning amount:
sql
Copy code
SELECT ma.auctionid AS "Auction ID", 
       mi.description AS "Item Description", 
       ma.enddate AS "End Date", 
       COALESCE(mu.name, '==') AS "Winner Name", 
       COALESCE(MAX(mb.amount), 0) AS "Winning Amount"
FROM mb_auction ma
LEFT JOIN mb_bid mb ON mb.auctionid = ma.auctionid
LEFT JOIN mb_user mu ON mu.userid = ma.winner
JOIN mb_item mi ON mi.itemid = ma.itemid
WHERE ma.status IN ('pending', 'sold', 'unsold')
GROUP BY ma.auctionid, mi.description, ma.enddate, mu.name;
These revisions add some distinct structure while ensuring they comply with the task description. Let me know if you need any further modifications!




SELECT ma.auctionid, 
       mi.description, 
       ma.enddate, 
       CASE 
           WHEN mu.name IS NULL THEN '==' 
           ELSE mu.name 
       END AS winner_name, 
       CASE 
           WHEN MAX(mb.amount) IS NULL THEN 0 
           ELSE MAX(mb.amount) 
       END AS winning_amount
FROM mb_auction ma
LEFT JOIN mb_bid mb ON mb.auctionid = ma.auctionid
LEFT JOIN mb_user mu ON mu.userid = ma.winner
JOIN mb_item mi ON mi.itemid = ma.itemid
WHERE ma.status IN ('pending', 'sold', 'unsold')
GROUP BY ma.auctionid, mi.description, ma.enddate, mu.name;



