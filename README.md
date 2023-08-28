SELECT
    u.id AS user_id,
    COALESCE(SUM(pr.purchased_price), 0) AS ltv
FROM
    Users u
LEFT JOIN
    unique_purchase_receipts pr ON u.id = pr.user_id
LEFT JOIN
    unique_purchase_orders upo ON pr.purchase_order_id = upo.id
WHERE
    (pr.deleted = FALSE OR pr.deleted IS NULL)
    AND (pr.is_subscription_trial = FALSE OR pr.is_subscription_trial IS NULL)
    AND (pr.is_free = FALSE OR pr.is_free IS NULL)
    AND (pr.trial_mode = FALSE OR pr.trial_mode IS NULL)
    AND (upo.deleted = FALSE OR upo.deleted IS NULL)
    AND (upo.is_free = FALSE OR upo.is_free IS NULL)
    AND (upo.is_trial = FALSE OR upo.is_trial IS NULL)
GROUP BY
    u.id
ORDER BY
    u.id ASC;
