#PoblarDimFecha

CREATE PROCEDURE PDF
@nk datetime 

AS 

BEGIN
	DECLARE @fid int
	DECLARE @fnk datetime
	DECLARE @fyear int = year(@nk)
	DECLARE @trimestre int
	DECLARE @fmes int
	DECLARE @mnombre nvarchar(15)
	DECLARE @mdia int
	DECLARE @sdia int
	DECLARE @dnombre nvarchar(15)
	DECLARE @yfiscal int
	DECLARE @tfiscal int
	DECLARE @finicio datetime
	DECLARE @ffin datetime

	SET @finicio = '01/01/2005'
	SET @ffin = @nk


WHILE @finicio <= @ffin
BEGIN
	SET @fid = PARSE(concat(year(@finicio),RIGHT('00' + CAST(DATEPART(mm, @finicio) AS varchar(2)), 2),RIGHT('00' + CAST(DATEPART(dd, @finicio) AS varchar(2)), 2)) AS INT )
	SET @fnk = @finicio
	SET @fyear = year(@finicio)
	SET @trimestre = datepart(quarter, @finicio)
	SET @fmes = month(@finicio)
	SET @mnombre = DateName(month,@finicio)
	SET @mdia = day(@finicio)
	SET @sdia = datepart(dw, @finicio)
	SET @dnombre = DateName(weekday,@finicio)
	
	IF month(@finicio) IN (1,2,3)
	BEGIN
	set @tfiscal = 3 
	set @yfiscal = year(@finicio)
	END
	ELSE
	BEGIN
		IF month(@finicio) IN (4,5,6)
		BEGIN
		set @tfiscal = 4
		set @yfiscal = year(@finicio)
		END
		ELSE 
		BEGIN
			IF month(@finicio) IN (7,8,9)
			BEGIN
			set @tfiscal = 1
			set @yfiscal = year(@finicio)+'1'
			END
			ELSE
			BEGIN 
			set @tfiscal = 2
			set @yfiscal = year(@finicio)+'1'
			END
		END
	END
			
INSERT INTO DimFecha(
		Fecha_ID,
		Fecha_NK,
		Fecha_Year,
		Fecha_Trimestre,
		Fecha_Mes,
		Mes_Nombre,
		Mes_Dia,
		Semana_Dia,
		Dia_Nombre,
		Year_Fiscal,
		Trimestre_Fiscal)
    VALUES (
	@fid,
	@fnk,
	@fyear,
	@trimestre,
	@fmes,
	@mnombre,
	@mdia,
	@sdia,
	@dnombre,
	@yfiscal,
	@tfiscal)
 SET @finicio = DateAdd(d, 1, @finicio)
END
END
